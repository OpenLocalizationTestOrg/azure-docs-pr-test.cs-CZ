---
title: Store-sendgrid-Java-How-to-send-email-example
description: "Postup odesílání e-mailu pomocí sendgrid vám umožňuje z prostředí Java v Azure nasazení"
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
ms.openlocfilehash: d80d7d9c54bad12a4d26d8623eeccdf9bc2a743a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a><span data-ttu-id="a5f10-103">Postup odesílání e-mailu pomocí sendgrid vám umožňuje z prostředí Java v Azure nasazení</span><span class="sxs-lookup"><span data-stu-id="a5f10-103">How to Send Email Using SendGrid from Java in an Azure Deployment</span></span>
<span data-ttu-id="a5f10-104">Následující příklad ukazuje, jak můžete sendgrid vám umožňuje použít k odesílání e-mailů z webové stránky hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="a5f10-104">The following example shows you how you can use SendGrid to send emails from a web page hosted in Azure.</span></span> <span data-ttu-id="a5f10-105">Výsledné aplikace vyzve uživatele, hodnoty e-mailu, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="a5f10-105">The resulting application will prompt the user for email values, as shown in the following screen shot.</span></span>

![E-mailu formuláře][emailform]

<span data-ttu-id="a5f10-107">Výsledný e-mailu, bude vypadat podobně jako na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="a5f10-107">The resulting email will look similar to the following screen shot.</span></span>

![E-mailové zprávy][emailsent]

<span data-ttu-id="a5f10-109">Budete muset následujícím postupem použít kód v tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="a5f10-109">You'll need to do the following to use the code in this topic:</span></span>

1. <span data-ttu-id="a5f10-110">Získat javax.mail JAR, například z <http://www.oracle.com/technetwork/java/javamail/index.html>.</span><span class="sxs-lookup"><span data-stu-id="a5f10-110">Obtain the javax.mail JARs, for example from <http://www.oracle.com/technetwork/java/javamail/index.html>.</span></span>
2. <span data-ttu-id="a5f10-111">Přidáte JAR do vaší cesta sestavení Java.</span><span class="sxs-lookup"><span data-stu-id="a5f10-111">Add the JARs to your Java build path.</span></span>
3. <span data-ttu-id="a5f10-112">Pokud používáte Eclipse k vytvoření této aplikace Java, můžete zahrnout knihovny sendgrid vám umožňuje v souboru nasazení vaší aplikace (WAR), pomocí funkce sestavení Eclipse na nasazení.</span><span class="sxs-lookup"><span data-stu-id="a5f10-112">If you are using Eclipse to create this Java application, you can include the SendGrid libraries in your application deployment file (WAR) using Eclipse's deployment assembly feature.</span></span> <span data-ttu-id="a5f10-113">Pokud nepoužíváte Eclipse k vytvoření této aplikace Java, zkontrolujte v knihovnách jsou zahrnuty v rámci stejné Azure role jako aplikace v jazyce Java a přidat do třídy cestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="a5f10-113">If you are not using Eclipse to create this Java application, ensure the libraries are included within the same Azure role as your Java application, and added to the class path of your application.</span></span>

<span data-ttu-id="a5f10-114">Musí také mít vlastní sendgrid vám umožňuje uživatelské jméno a heslo, abyste mohli odeslat e-mailu.</span><span class="sxs-lookup"><span data-stu-id="a5f10-114">You must also have your own SendGrid username and password, to be able to send the email.</span></span> <span data-ttu-id="a5f10-115">Začínáme s Sendgridu, najdete v tématu [postup odesílání e-mailu pomocí sendgrid vám umožňuje z prostředí Java](store-sendgrid-java-how-to-send-email.md).</span><span class="sxs-lookup"><span data-stu-id="a5f10-115">To get started with SendGrid, see [How to send email using SendGrid from Java](store-sendgrid-java-how-to-send-email.md).</span></span>

<span data-ttu-id="a5f10-116">Kromě toho znalost na informace na [vytvoření aplikace Hello World služby Azure v prostředí Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944), nebo s jinými technikami, pro hostování aplikací Java v Azure, pokud nepoužíváte Eclipse, důrazně doporučujeme.</span><span class="sxs-lookup"><span data-stu-id="a5f10-116">Additionally, familiarity with the information at [Creating a Hello World Application for Azure in Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944), or with other techniques for hosting Java applications in Azure if you are not using Eclipse, is highly recommended.</span></span>

## <a name="create-a-web-form-for-sending-email"></a><span data-ttu-id="a5f10-117">Vytvoření webového formuláře pro odesílání e-mailu</span><span class="sxs-lookup"><span data-stu-id="a5f10-117">Create a web form for sending email</span></span>
<span data-ttu-id="a5f10-118">Následující kód ukazuje, jak vytvořit webového formuláře pro načtení dat uživatele pro odesílání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="a5f10-118">The following code shows how to create a web form to retrieve user data for sending email.</span></span> <span data-ttu-id="a5f10-119">Pro účely tohoto obsahu, má název souboru JSP **emailform.jsp**.</span><span class="sxs-lookup"><span data-stu-id="a5f10-119">For purposes of this content, the JSP file is named **emailform.jsp**.</span></span>

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

## <a name="create-the-code-to-send-the-email"></a><span data-ttu-id="a5f10-120">Vytvoření kódu pro odeslání e-mailu</span><span class="sxs-lookup"><span data-stu-id="a5f10-120">Create the code to send the email</span></span>
<span data-ttu-id="a5f10-121">Následující kód, který je volána po vyplnění formuláře v emailform.jsp, vytvoří e-mailové zprávy a odešle ji.</span><span class="sxs-lookup"><span data-stu-id="a5f10-121">The following code, which is called when you complete the form in emailform.jsp, creates the email message and sends it.</span></span> <span data-ttu-id="a5f10-122">Pro účely tohoto obsahu, má název souboru JSP **sendemail.jsp**.</span><span class="sxs-lookup"><span data-stu-id="a5f10-122">For purposes of this content, the JSP file is named **sendemail.jsp**.</span></span>

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

         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";

         Properties properties;

         properties = new Properties();

         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");

         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");

         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();

         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);

         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);

         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 

         message = new MimeMessage(mailSession);

         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            

         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);

         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");

         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");

         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
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

<span data-ttu-id="a5f10-123">Kromě odesílání e-mailu, poskytuje emailform.jsp výsledku pro uživatele. na následujícím snímku obrazovky je například:</span><span class="sxs-lookup"><span data-stu-id="a5f10-123">In addition to sending the email, emailform.jsp provides a result for the user; an example is the following screen shot:</span></span>

![Odesílání e-mailu výsledek][emailresult]

## <a name="next-steps"></a><span data-ttu-id="a5f10-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5f10-125">Next steps</span></span>
<span data-ttu-id="a5f10-126">Nasazení aplikace emulátoru služby výpočty a prohlížeče, spusťte emailform.jsp, zadejte hodnoty ve formátu, klikněte na tlačítko **odeslání této e-mailu**a pak zobrazte výsledky v sendemail.jsp.</span><span class="sxs-lookup"><span data-stu-id="a5f10-126">Deploy your application to the compute emulator and within a browser run emailform.jsp, enter values in the form, click **Send this email**, and then see results in sendemail.jsp.</span></span>

<span data-ttu-id="a5f10-127">Tento kód byl poskytnut návod, jak používat sendgrid vám umožňuje v jazyce Java v Azure.</span><span class="sxs-lookup"><span data-stu-id="a5f10-127">This code was provided to show you how to use SendGrid in Java on Azure.</span></span> <span data-ttu-id="a5f10-128">Před nasazením do Azure v produkčním prostředí, můžete přidat další zpracování chyb a další funkce.</span><span class="sxs-lookup"><span data-stu-id="a5f10-128">Before deploying to Azure in production, you may want to add more error handling or other features.</span></span> <span data-ttu-id="a5f10-129">Například:</span><span class="sxs-lookup"><span data-stu-id="a5f10-129">For example:</span></span> 

* <span data-ttu-id="a5f10-130">Můžete použít objekty BLOB úložiště Azure nebo databáze SQL pro ukládání e-mailové adresy a e-mailové zprávy, místo použití webového formuláře.</span><span class="sxs-lookup"><span data-stu-id="a5f10-130">You could use Azure storage blobs or SQL Database to store email addresses and email messages, instead of using a web form.</span></span> <span data-ttu-id="a5f10-131">Informace o použití objektů BLOB služby Azure storage v jazyce Java najdete v tématu [jak používat služby úložiště objektů Blob z Javy](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/).</span><span class="sxs-lookup"><span data-stu-id="a5f10-131">For information about using Azure storage blobs in Java, see [How to Use the Blob Storage Service from Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/).</span></span> <span data-ttu-id="a5f10-132">Informace o používání databáze SQL v jazyce Java najdete v tématu [pomocí SQL Database v jazyce Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).</span><span class="sxs-lookup"><span data-stu-id="a5f10-132">For information about using SQL Database in Java, see [Using SQL Database in Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).</span></span>
* <span data-ttu-id="a5f10-133">Můžete použít `RoleEnvironment.getConfigurationSettings` načíst sendgrid vám umožňuje uživatelské jméno a heslo z nastavení konfigurace vašeho nasazení, místo použití webového formuláře pro načtení těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="a5f10-133">You could use `RoleEnvironment.getConfigurationSettings` to retrieve the SendGrid username and password from your deployment's configuration settings, instead of using the web form to retrieve those values.</span></span> <span data-ttu-id="a5f10-134">Informace o `RoleEnvironment` třídy najdete v tématu [pomocí běhové knihovny služby Azure v JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) a v dokumentaci k balíčku běh služby Azure na <http://dl.windowsazure.com/javadoc>.</span><span class="sxs-lookup"><span data-stu-id="a5f10-134">For information about the `RoleEnvironment` class, see [Using the Azure Service Runtime Library in JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) and the Azure Service Runtime package documentation at <http://dl.windowsazure.com/javadoc>.</span></span>
* <span data-ttu-id="a5f10-135">Další informace o používání sendgrid vám umožňuje v jazyce Java najdete v tématu [postup odesílání e-mailu pomocí sendgrid vám umožňuje z prostředí Java](store-sendgrid-java-how-to-send-email.md).</span><span class="sxs-lookup"><span data-stu-id="a5f10-135">For more information about using SendGrid in Java, see [How to send email using SendGrid from Java](store-sendgrid-java-how-to-send-email.md).</span></span>

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
