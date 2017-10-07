---
title: "aaaHow tooMake telefonní hovor z Twilio (Java) | Microsoft Docs"
description: "Zjistěte, jak toomake telefonní hovor z webové stránky v aplikaci Java v Azure pomocí Twilio."
services: 
documentationcenter: java
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 0381789e-e775-41a0-a784-294275192b1d
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 04fe5a78d431a79790dee3ca75c2b004aea4345d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomake-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Jak tooMake Twilio pomocí telefonního hovoru v aplikaci Java v Azure
Hello následující příklad ukazuje, je použití Twilio toomake volání z webové stránky hostované v Azure. Výsledná aplikace Hello vyzve uživatele hello hodnoty telefonní hovor, jak ukazuje následující snímek obrazovky hello.

![Azure volání formulář s využitím Twilio a Java][twilio_java]

Budete potřebovat následující hello toodo toouse hello kód v tomto tématu:

1. Získání účtu Twilio a ověření tokenu. začít s Twilio, tooget vyhodnotit ceny v [http://www.twilio.com/pricing][twilio_pricing]. Můžete si zaregistrovat v [https://www.twilio.com/try-twilio][try_twilio]. Informace o rozhraní API poskytované Twilio hello najdete v tématu [http://www.twilio.com/api][twilio_api].
2. Získáte hello Twilio JAR. V [https://github.com/twilio/twilio-java][twilio_java_github], můžete stáhnout hello Githubu zdroje a vytvořit vlastní JAR nebo stáhnout předdefinovaných JAR (s nebo bez závislosti).
   Hello kód v tomto tématu, byla zapsána pomocí hello předem vytvořené JAR TwilioJava 3.3.8 s závislosti.
3. Přidejte tooyour JAR hello cesta sestavení Java.
4. Pokud používáte Eclipse toocreate tuto aplikaci Java, zahrňte do souboru nasazení vaší aplikace (WAR), pomocí funkce sestavení nasazení na Eclipse hello Twilio JAR. Pokud nepoužíváte Eclipse toocreate tuto aplikaci Java, ujistěte se, je součástí hello hello Twilio JAR stejné Azure role jako cestu – třída aplikace a přidané toohello vaší Java vaší aplikace.
5. Ujistěte se, úložiště klíčů vaší cacerts obsahuje hello certifikát Equifax zabezpečení certifikační autority s MD5 otisk prstu 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello sériové číslo je 35:DE:F4:CF a D2:32:09:AD:23:D hello SHA1 otisk prstu 3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Toto je hello certifikát certifikační autority certifikátu pro hello [https://api.twilio.com] [ twilio_api_service] služba, která je volána, když používáte rozhraní API Twilio. Informace o přidání této certifikační Autority certifikátu tooyour JDK na cacert úložiště najdete v tématu [přidání certifikátu toohello, úložiště certifikátů certifikační Autority Java][add_ca_cert].

Kromě toho znalost hello informace [vytváření hello Hello World aplikace pomocí nástrojů Azure pro Eclipse][azure_java_eclipse_hello_world], nebo s jinými technikami pro hostování aplikací Java v Azure, pokud jste nepoužívají Eclipse, důrazně doporučujeme.

## <a name="create-a-web-form-for-making-a-call"></a>Vytvoření webového formuláře pro volání
Hello následující kód ukazuje, jak toocreate webové formuláři tooretrieve uživatelská data pro volání. Pro účely tohoto příkladu nového dynamického webového projektu, s názvem **TwilioCloud**, byl vytvořen, a **callform.jsp** byl přidán jako soubor JSP.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is hello call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-hello-code-toomake-hello-call"></a>Vytvoření hello kód toomake hello volání
Hello následující kód, který je volána, když uživatel hello dokončí hello formulář zobrazuje callform.jsp, vytvoří zprávu volání hello a vygeneruje hello volání. Pro účely tohoto příkladu je soubor JSP hello s názvem **makecall.jsp** a byla přidána toohello **TwilioCloud** projektu. (Použití vašeho účtu Twilio a ověřování tokenu místo hello zástupné hodnoty přiřazené příliš**accountSID** a **ověřovacího tokenu** v hello kód níže.)

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of hello placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";

         // Instantiate an instance of hello Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve hello account, used later tooretrieve hello CallFactory.
         Account account = client.getAccount();

         // Display hello client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");

         // Display hello API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");

         // Retrieve hello values entered by hello user.
         String callTo = request.getParameter("callTo");  
         // hello Outgoing Caller ID, used for hello From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");

         // Replace spaces in hello user's text with '%20', 
         // toomake hello text suitable for a URL.
         userText = userText.replace(" ", "%20");

         // Create a URL using hello Twilio message and hello user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;

         // Display hello message URL.
         out.println("<p>");
         out.println("hello URL is " + Url);
         out.println("</p>");

         // Place hello call From, tooand URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);

         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Kromě toho toomaking hello volání, makecall.jsp zobrazí hello Twilio koncový bod, verze rozhraní API a stav volání hello. Příkladem je hello následující snímek obrazovky:

![Azure volání odpovědi pomocí Twilio a Java][twilio_java_response]

## <a name="run-hello-application"></a>Spuštění aplikace hello
Následující jsou základní kroky toorun hello aplikace; Podrobnosti o těchto krocích naleznete v [vytváření hello Hello World aplikace pomocí nástrojů Azure pro Eclipse][azure_java_eclipse_hello_world].

1. Export vašeho TwilioCloud WAR toohello Azure **approot** složky. 
2. Upravit **startup.cmd** toounzip vaše TwilioCloud WAR.
3. Zkompilujte aplikaci pro emulátoru služby výpočty hello.
4. Spuštění nasazení v emulátoru služby výpočty hello.
5. Otevřete prohlížeč a spusťte **http://localhost:8080/TwilioCloud/callform.jsp**.
6. Zadejte hodnoty ve formuláři hello, klikněte na tlačítko **toto volání**a pak zobrazte výsledky hello v makecall.jsp.

Pokud jste připravené toodeploy tooAzure, znovu zkompiluje pro cloud toohello nasazení, tooAzure nasazení a spuštění http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp v prohlížeči hello (nahraďte hodnota pro *your_hosted_name*).

## <a name="next-steps"></a>Další kroky
Tento kód byl poskytnut tooshow je základní funkce pomocí Twilio v jazyce Java v Azure. Před nasazením tooAzure v produkčním prostředí, můžete chtít tooadd další zpracování chyb a další funkce. Například:

* Místo použití webového formuláře, můžete použít objekty BLOB úložiště Azure nebo databázi SQL toostore telefonních čísel a volání text. Informace o použití objektů BLOB služby Azure storage v jazyce Java najdete v tématu [jak tooUse hello služby úložiště objektů Blob z Javy][howto_blob_storage_java]. Informace o používání databáze SQL v jazyce Java najdete v tématu [pomocí SQL Database v jazyce Java][howto_sql_azure_java].
* Můžete použít **RoleEnvironment.getConfigurationSettings** ID účtu Twilio hello tooretrieve a ověřování tokenu z nastavení konfigurace vašeho nasazení, místo pevně kódováno hello hodnoty v makecall.jsp. Informace o hello **RoleEnvironment** třídy najdete v tématu [hello pomocí knihovny spuštění služby Azure v JSP] [ azure_runtime_jsp] a dokumentaci balíček hello běh služby Azure v [http://dl.windowsazure.com/javadoc][azure_javadoc].
* Kód makecall.jsp Hello přiřadí URL poskytnutou Twilio, [http://twimlets.com/message][twimlet_message_url], toohello **Url** proměnné. Tato adresa URL obsahuje odpovědi Twilio Markup Language (TwiML), která informuje o tom, jak volat tooproceed s hello Twilio. Například může obsahovat hello TwiML, která je vrácena  **&lt;indikované&gt;**  akci, která má za následek textová hodnota mluvené toohello volání příjemce. Místo použití hello URL poskytnutou Twilio, je sestavení vlastní tooTwilio toorespond služby požadavek; Další informace najdete v tématu [jak tooUse Twilio pro hlasový a možnosti serveru SMS v jazyce Java][howto_twilio_voice_sms_java]. Další informace o TwiML lze najít na [http://www.twilio.com/docs/api/twiml][twiml]a další informace o  **&lt;indikované&gt;**  a ostatní operace Twilio naleznete na adrese [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Přečtěte si pokyny pro zabezpečení Twilio hello v [https://www.twilio.com/docs/security][twilio_docs_security].

Další informace o Twilio najdete v tématu [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Viz také
* [Jak tooUse Twilio pro hlasový a možnosti serveru SMS v jazyce Java][howto_twilio_voice_sms_java]
* [Přidání certifikátu toohello úložiště certifikátů certifikační Autority Java][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
