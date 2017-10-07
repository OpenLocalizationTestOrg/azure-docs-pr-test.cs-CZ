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
# <a name="how-toomake-a-phone-call-using-twilio-in-a-java-application-on-azure"></a><span data-ttu-id="5f006-103">Jak tooMake Twilio pomocí telefonního hovoru v aplikaci Java v Azure</span><span class="sxs-lookup"><span data-stu-id="5f006-103">How tooMake a Phone Call Using Twilio in a Java Application on Azure</span></span>
<span data-ttu-id="5f006-104">Hello následující příklad ukazuje, je použití Twilio toomake volání z webové stránky hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="5f006-104">hello following example shows you how you can use Twilio toomake a call from a web page hosted in Azure.</span></span> <span data-ttu-id="5f006-105">Výsledná aplikace Hello vyzve uživatele hello hodnoty telefonní hovor, jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="5f006-105">hello resulting application will prompt hello user for phone call values, as shown in hello following screen shot.</span></span>

![Azure volání formulář s využitím Twilio a Java][twilio_java]

<span data-ttu-id="5f006-107">Budete potřebovat následující hello toodo toouse hello kód v tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="5f006-107">You'll need toodo hello following toouse hello code in this topic:</span></span>

1. <span data-ttu-id="5f006-108">Získání účtu Twilio a ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="5f006-108">Acquire a Twilio account and authentication token.</span></span> <span data-ttu-id="5f006-109">začít s Twilio, tooget vyhodnotit ceny v [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="5f006-109">tooget started with Twilio, evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="5f006-110">Můžete si zaregistrovat v [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="5f006-110">You can sign up at [https://www.twilio.com/try-twilio][try_twilio].</span></span> <span data-ttu-id="5f006-111">Informace o rozhraní API poskytované Twilio hello najdete v tématu [http://www.twilio.com/api][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="5f006-111">For information about hello API provided by Twilio, see [http://www.twilio.com/api][twilio_api].</span></span>
2. <span data-ttu-id="5f006-112">Získáte hello Twilio JAR.</span><span class="sxs-lookup"><span data-stu-id="5f006-112">Obtain hello Twilio JAR.</span></span> <span data-ttu-id="5f006-113">V [https://github.com/twilio/twilio-java][twilio_java_github], můžete stáhnout hello Githubu zdroje a vytvořit vlastní JAR nebo stáhnout předdefinovaných JAR (s nebo bez závislosti).</span><span class="sxs-lookup"><span data-stu-id="5f006-113">At [https://github.com/twilio/twilio-java][twilio_java_github], you can download hello GitHub sources and create your own JAR, or download a pre-built JAR (with or without dependencies).</span></span>
   <span data-ttu-id="5f006-114">Hello kód v tomto tématu, byla zapsána pomocí hello předem vytvořené JAR TwilioJava 3.3.8 s závislosti.</span><span class="sxs-lookup"><span data-stu-id="5f006-114">hello code in this topic was written using hello pre-built TwilioJava-3.3.8-with-dependencies JAR.</span></span>
3. <span data-ttu-id="5f006-115">Přidejte tooyour JAR hello cesta sestavení Java.</span><span class="sxs-lookup"><span data-stu-id="5f006-115">Add hello JAR tooyour Java build path.</span></span>
4. <span data-ttu-id="5f006-116">Pokud používáte Eclipse toocreate tuto aplikaci Java, zahrňte do souboru nasazení vaší aplikace (WAR), pomocí funkce sestavení nasazení na Eclipse hello Twilio JAR.</span><span class="sxs-lookup"><span data-stu-id="5f006-116">If you are using Eclipse toocreate this Java application, include hello Twilio JAR in your application deployment file (WAR) using Eclipse's deployment assembly feature.</span></span> <span data-ttu-id="5f006-117">Pokud nepoužíváte Eclipse toocreate tuto aplikaci Java, ujistěte se, je součástí hello hello Twilio JAR stejné Azure role jako cestu – třída aplikace a přidané toohello vaší Java vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f006-117">If you are not using Eclipse toocreate this Java application, ensure hello Twilio JAR is included within hello same Azure role as your Java application, and added toohello class path of your application.</span></span>
5. <span data-ttu-id="5f006-118">Ujistěte se, úložiště klíčů vaší cacerts obsahuje hello certifikát Equifax zabezpečení certifikační autority s MD5 otisk prstu 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello sériové číslo je 35:DE:F4:CF a D2:32:09:AD:23:D hello SHA1 otisk prstu 3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A).</span><span class="sxs-lookup"><span data-stu-id="5f006-118">Ensure your cacerts keystore contains hello Equifax Secure Certificate Authority certificate with MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello serial number is 35:DE:F4:CF and hello SHA1 fingerprint is D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A).</span></span> <span data-ttu-id="5f006-119">Toto je hello certifikát certifikační autority certifikátu pro hello [https://api.twilio.com] [ twilio_api_service] služba, která je volána, když používáte rozhraní API Twilio.</span><span class="sxs-lookup"><span data-stu-id="5f006-119">This is hello certificate authority (CA) certificate for hello [https://api.twilio.com][twilio_api_service] service, which is called when you use Twilio APIs.</span></span> <span data-ttu-id="5f006-120">Informace o přidání této certifikační Autority certifikátu tooyour JDK na cacert úložiště najdete v tématu [přidání certifikátu toohello, úložiště certifikátů certifikační Autority Java][add_ca_cert].</span><span class="sxs-lookup"><span data-stu-id="5f006-120">For information about adding this CA certificate tooyour JDK's cacert store, see [Adding a Certificate toohello Java CA Certificate Store][add_ca_cert].</span></span>

<span data-ttu-id="5f006-121">Kromě toho znalost hello informace [vytváření hello Hello World aplikace pomocí nástrojů Azure pro Eclipse][azure_java_eclipse_hello_world], nebo s jinými technikami pro hostování aplikací Java v Azure, pokud jste nepoužívají Eclipse, důrazně doporučujeme.</span><span class="sxs-lookup"><span data-stu-id="5f006-121">Additionally, familiarity with hello information at [Creating a Hello World Application Using hello Azure Toolkit for Eclipse][azure_java_eclipse_hello_world], or with other techniques for hosting Java applications in Azure if you are not using Eclipse, is highly recommended.</span></span>

## <a name="create-a-web-form-for-making-a-call"></a><span data-ttu-id="5f006-122">Vytvoření webového formuláře pro volání</span><span class="sxs-lookup"><span data-stu-id="5f006-122">Create a web form for making a call</span></span>
<span data-ttu-id="5f006-123">Hello následující kód ukazuje, jak toocreate webové formuláři tooretrieve uživatelská data pro volání.</span><span class="sxs-lookup"><span data-stu-id="5f006-123">hello following code shows how toocreate a web form tooretrieve user data for making a call.</span></span> <span data-ttu-id="5f006-124">Pro účely tohoto příkladu nového dynamického webového projektu, s názvem **TwilioCloud**, byl vytvořen, a **callform.jsp** byl přidán jako soubor JSP.</span><span class="sxs-lookup"><span data-stu-id="5f006-124">For purposes of this example, a new dynamic web project, named **TwilioCloud**, was created, and **callform.jsp** was added as a JSP file.</span></span>

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

## <a name="create-hello-code-toomake-hello-call"></a><span data-ttu-id="5f006-125">Vytvoření hello kód toomake hello volání</span><span class="sxs-lookup"><span data-stu-id="5f006-125">Create hello code toomake hello call</span></span>
<span data-ttu-id="5f006-126">Hello následující kód, který je volána, když uživatel hello dokončí hello formulář zobrazuje callform.jsp, vytvoří zprávu volání hello a vygeneruje hello volání.</span><span class="sxs-lookup"><span data-stu-id="5f006-126">hello following code, which is called when hello user completes hello form displayed by callform.jsp, creates hello call message and generates hello call.</span></span> <span data-ttu-id="5f006-127">Pro účely tohoto příkladu je soubor JSP hello s názvem **makecall.jsp** a byla přidána toohello **TwilioCloud** projektu.</span><span class="sxs-lookup"><span data-stu-id="5f006-127">For purposes of this example, hello JSP file is named **makecall.jsp** and was added toohello **TwilioCloud** project.</span></span> <span data-ttu-id="5f006-128">(Použití vašeho účtu Twilio a ověřování tokenu místo hello zástupné hodnoty přiřazené příliš**accountSID** a **ověřovacího tokenu** v hello kód níže.)</span><span class="sxs-lookup"><span data-stu-id="5f006-128">(Use your Twilio account and authentication token instead of hello placeholder values assigned too**accountSID** and **authToken** in hello code below.)</span></span>

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

<span data-ttu-id="5f006-129">Kromě toho toomaking hello volání, makecall.jsp zobrazí hello Twilio koncový bod, verze rozhraní API a stav volání hello.</span><span class="sxs-lookup"><span data-stu-id="5f006-129">In addition toomaking hello call, makecall.jsp displays hello Twilio endpoint, API version, and hello call status.</span></span> <span data-ttu-id="5f006-130">Příkladem je hello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="5f006-130">An example is hello following screen shot:</span></span>

![Azure volání odpovědi pomocí Twilio a Java][twilio_java_response]

## <a name="run-hello-application"></a><span data-ttu-id="5f006-132">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="5f006-132">Run hello application</span></span>
<span data-ttu-id="5f006-133">Následující jsou základní kroky toorun hello aplikace; Podrobnosti o těchto krocích naleznete v [vytváření hello Hello World aplikace pomocí nástrojů Azure pro Eclipse][azure_java_eclipse_hello_world].</span><span class="sxs-lookup"><span data-stu-id="5f006-133">Following are hello high-level steps toorun your application; details for these steps can be found at [Creating a Hello World Application Using hello Azure Toolkit for Eclipse][azure_java_eclipse_hello_world].</span></span>

1. <span data-ttu-id="5f006-134">Export vašeho TwilioCloud WAR toohello Azure **approot** složky.</span><span class="sxs-lookup"><span data-stu-id="5f006-134">Export your TwilioCloud WAR toohello Azure **approot** folder.</span></span> 
2. <span data-ttu-id="5f006-135">Upravit **startup.cmd** toounzip vaše TwilioCloud WAR.</span><span class="sxs-lookup"><span data-stu-id="5f006-135">Modify **startup.cmd** toounzip your TwilioCloud WAR.</span></span>
3. <span data-ttu-id="5f006-136">Zkompilujte aplikaci pro emulátoru služby výpočty hello.</span><span class="sxs-lookup"><span data-stu-id="5f006-136">Compile your application for hello compute emulator.</span></span>
4. <span data-ttu-id="5f006-137">Spuštění nasazení v emulátoru služby výpočty hello.</span><span class="sxs-lookup"><span data-stu-id="5f006-137">Start your deployment in hello compute emulator.</span></span>
5. <span data-ttu-id="5f006-138">Otevřete prohlížeč a spusťte **http://localhost:8080/TwilioCloud/callform.jsp**.</span><span class="sxs-lookup"><span data-stu-id="5f006-138">Open a browser, and run **http://localhost:8080/TwilioCloud/callform.jsp**.</span></span>
6. <span data-ttu-id="5f006-139">Zadejte hodnoty ve formuláři hello, klikněte na tlačítko **toto volání**a pak zobrazte výsledky hello v makecall.jsp.</span><span class="sxs-lookup"><span data-stu-id="5f006-139">Enter values in hello form, click **Make this call**, and then see hello results in makecall.jsp.</span></span>

<span data-ttu-id="5f006-140">Pokud jste připravené toodeploy tooAzure, znovu zkompiluje pro cloud toohello nasazení, tooAzure nasazení a spuštění http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp v prohlížeči hello (nahraďte hodnota pro *your_hosted_name*).</span><span class="sxs-lookup"><span data-stu-id="5f006-140">When you are ready toodeploy tooAzure, recompile for deployment toohello cloud, deploy tooAzure, and run http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp in hello browser (substitute your value for *your_hosted_name*).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5f006-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5f006-141">Next steps</span></span>
<span data-ttu-id="5f006-142">Tento kód byl poskytnut tooshow je základní funkce pomocí Twilio v jazyce Java v Azure.</span><span class="sxs-lookup"><span data-stu-id="5f006-142">This code was provided tooshow you basic functionality using Twilio in Java on Azure.</span></span> <span data-ttu-id="5f006-143">Před nasazením tooAzure v produkčním prostředí, můžete chtít tooadd další zpracování chyb a další funkce.</span><span class="sxs-lookup"><span data-stu-id="5f006-143">Before deploying tooAzure in production, you may want tooadd more error handling or other features.</span></span> <span data-ttu-id="5f006-144">Například:</span><span class="sxs-lookup"><span data-stu-id="5f006-144">For example:</span></span>

* <span data-ttu-id="5f006-145">Místo použití webového formuláře, můžete použít objekty BLOB úložiště Azure nebo databázi SQL toostore telefonních čísel a volání text.</span><span class="sxs-lookup"><span data-stu-id="5f006-145">Instead of using a web form, you could use Azure storage blobs or SQL Database toostore phone numbers and call text.</span></span> <span data-ttu-id="5f006-146">Informace o použití objektů BLOB služby Azure storage v jazyce Java najdete v tématu [jak tooUse hello služby úložiště objektů Blob z Javy][howto_blob_storage_java].</span><span class="sxs-lookup"><span data-stu-id="5f006-146">For information about using Azure storage blobs in Java, see [How tooUse hello Blob Storage Service from Java][howto_blob_storage_java].</span></span> <span data-ttu-id="5f006-147">Informace o používání databáze SQL v jazyce Java najdete v tématu [pomocí SQL Database v jazyce Java][howto_sql_azure_java].</span><span class="sxs-lookup"><span data-stu-id="5f006-147">For information about using SQL Database in Java, see [Using SQL Database in Java][howto_sql_azure_java].</span></span>
* <span data-ttu-id="5f006-148">Můžete použít **RoleEnvironment.getConfigurationSettings** ID účtu Twilio hello tooretrieve a ověřování tokenu z nastavení konfigurace vašeho nasazení, místo pevně kódováno hello hodnoty v makecall.jsp.</span><span class="sxs-lookup"><span data-stu-id="5f006-148">You could use **RoleEnvironment.getConfigurationSettings** tooretrieve hello Twilio account ID and authentication token from your deployment's configuration settings, instead of hard-coding hello values in makecall.jsp.</span></span> <span data-ttu-id="5f006-149">Informace o hello **RoleEnvironment** třídy najdete v tématu [hello pomocí knihovny spuštění služby Azure v JSP] [ azure_runtime_jsp] a dokumentaci balíček hello běh služby Azure v [http://dl.windowsazure.com/javadoc][azure_javadoc].</span><span class="sxs-lookup"><span data-stu-id="5f006-149">For information about hello **RoleEnvironment** class, see [Using hello Azure Service Runtime Library in JSP][azure_runtime_jsp] and hello Azure Service Runtime package documentation at [http://dl.windowsazure.com/javadoc][azure_javadoc].</span></span>
* <span data-ttu-id="5f006-150">Kód makecall.jsp Hello přiřadí URL poskytnutou Twilio, [http://twimlets.com/message][twimlet_message_url], toohello **Url** proměnné.</span><span class="sxs-lookup"><span data-stu-id="5f006-150">hello makecall.jsp code assigns a Twilio-provided URL, [http://twimlets.com/message][twimlet_message_url], toohello **Url** variable.</span></span> <span data-ttu-id="5f006-151">Tato adresa URL obsahuje odpovědi Twilio Markup Language (TwiML), která informuje o tom, jak volat tooproceed s hello Twilio.</span><span class="sxs-lookup"><span data-stu-id="5f006-151">This URL provides a Twilio Markup Language (TwiML) response that informs Twilio how tooproceed with hello call.</span></span> <span data-ttu-id="5f006-152">Například může obsahovat hello TwiML, která je vrácena  **&lt;indikované&gt;**  akci, která má za následek textová hodnota mluvené toohello volání příjemce.</span><span class="sxs-lookup"><span data-stu-id="5f006-152">For example, hello TwiML that is returned can contain a **&lt;Say&gt;** verb that results in text being spoken toohello call recipient.</span></span> <span data-ttu-id="5f006-153">Místo použití hello URL poskytnutou Twilio, je sestavení vlastní tooTwilio toorespond služby požadavek; Další informace najdete v tématu [jak tooUse Twilio pro hlasový a možnosti serveru SMS v jazyce Java][howto_twilio_voice_sms_java].</span><span class="sxs-lookup"><span data-stu-id="5f006-153">Instead of using hello Twilio-provided URL, you could build your own service toorespond tooTwilio's request; for more information, see [How tooUse Twilio for Voice and SMS Capabilities in Java][howto_twilio_voice_sms_java].</span></span> <span data-ttu-id="5f006-154">Další informace o TwiML lze najít na [http://www.twilio.com/docs/api/twiml][twiml]a další informace o  **&lt;indikované&gt;**  a ostatní operace Twilio naleznete na adrese [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="5f006-154">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml], and more information about **&lt;Say&gt;** and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>
* <span data-ttu-id="5f006-155">Přečtěte si pokyny pro zabezpečení Twilio hello v [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="5f006-155">Read hello Twilio security guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>

<span data-ttu-id="5f006-156">Další informace o Twilio najdete v tématu [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="5f006-156">For additional information about Twilio, see [https://www.twilio.com/docs][twilio_docs].</span></span>

## <a name="see-also"></a><span data-ttu-id="5f006-157">Viz také</span><span class="sxs-lookup"><span data-stu-id="5f006-157">See Also</span></span>
* <span data-ttu-id="5f006-158">[Jak tooUse Twilio pro hlasový a možnosti serveru SMS v jazyce Java][howto_twilio_voice_sms_java]</span><span class="sxs-lookup"><span data-stu-id="5f006-158">[How tooUse Twilio for Voice and SMS Capabilities in Java][howto_twilio_voice_sms_java]</span></span>
* <span data-ttu-id="5f006-159">[Přidání certifikátu toohello úložiště certifikátů certifikační Autority Java][add_ca_cert]</span><span class="sxs-lookup"><span data-stu-id="5f006-159">[Adding a Certificate toohello Java CA Certificate Store][add_ca_cert]</span></span>

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
