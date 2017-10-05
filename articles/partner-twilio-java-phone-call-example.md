---
title: "Postup telefonní hovor z Twilio (Java) | Microsoft Docs"
description: "Naučte se telefonní hovor z webové stránky v aplikaci Java v Azure pomocí Twilio."
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
ms.openlocfilehash: 04ecb80a2a9e15b549b47138caf71c7e64bda500
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a><span data-ttu-id="a227e-103">Postup telefonní hovor pomocí Twilio v aplikaci Java v Azure</span><span class="sxs-lookup"><span data-stu-id="a227e-103">How to Make a Phone Call Using Twilio in a Java Application on Azure</span></span>
<span data-ttu-id="a227e-104">Následující příklad ukazuje, jak Twilio můžete využít volání z webové stránky hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="a227e-104">The following example shows you how you can use Twilio to make a call from a web page hosted in Azure.</span></span> <span data-ttu-id="a227e-105">Výsledné aplikace vyzve uživatele, hodnoty telefonní hovor, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="a227e-105">The resulting application will prompt the user for phone call values, as shown in the following screen shot.</span></span>

![Azure volání formulář s využitím Twilio a Java][twilio_java]

<span data-ttu-id="a227e-107">Budete muset následujícím postupem použít kód v tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="a227e-107">You'll need to do the following to use the code in this topic:</span></span>

1. <span data-ttu-id="a227e-108">Získání účtu Twilio a ověření tokenu.</span><span class="sxs-lookup"><span data-stu-id="a227e-108">Acquire a Twilio account and authentication token.</span></span> <span data-ttu-id="a227e-109">Začít s Twilio vyhodnotit ceny v [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="a227e-109">To get started with Twilio, evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="a227e-110">Můžete si zaregistrovat v [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="a227e-110">You can sign up at [https://www.twilio.com/try-twilio][try_twilio].</span></span> <span data-ttu-id="a227e-111">Informace o rozhraní API poskytované Twilio najdete v tématu [http://www.twilio.com/api][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="a227e-111">For information about the API provided by Twilio, see [http://www.twilio.com/api][twilio_api].</span></span>
2. <span data-ttu-id="a227e-112">Získáte Twilio JAR.</span><span class="sxs-lookup"><span data-stu-id="a227e-112">Obtain the Twilio JAR.</span></span> <span data-ttu-id="a227e-113">V [https://github.com/twilio/twilio-java][twilio_java_github], můžete stáhnout zdroje Githubu a vytvořit vlastní JAR nebo stáhnout předdefinovaných JAR (s nebo bez závislosti).</span><span class="sxs-lookup"><span data-stu-id="a227e-113">At [https://github.com/twilio/twilio-java][twilio_java_github], you can download the GitHub sources and create your own JAR, or download a pre-built JAR (with or without dependencies).</span></span>
   <span data-ttu-id="a227e-114">Kód v tomto tématu, byla zapsána pomocí předdefinovaných JAR TwilioJava 3.3.8 s závislosti.</span><span class="sxs-lookup"><span data-stu-id="a227e-114">The code in this topic was written using the pre-built TwilioJava-3.3.8-with-dependencies JAR.</span></span>
3. <span data-ttu-id="a227e-115">Přidáte JAR do vaší cesta sestavení Java.</span><span class="sxs-lookup"><span data-stu-id="a227e-115">Add the JAR to your Java build path.</span></span>
4. <span data-ttu-id="a227e-116">Pokud používáte Eclipse k vytvoření této aplikace Java, zahrňte do souboru nasazení vaší aplikace (WAR), pomocí funkce sestavení nasazení na Eclipse Twilio JAR.</span><span class="sxs-lookup"><span data-stu-id="a227e-116">If you are using Eclipse to create this Java application, include the Twilio JAR in your application deployment file (WAR) using Eclipse's deployment assembly feature.</span></span> <span data-ttu-id="a227e-117">Pokud nepoužíváte Eclipse k vytvoření této aplikace Java, ujistěte se, Twilio JAR je uvedený v rámci stejné Azure role jako aplikace v jazyce Java a přidat do třídy cestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="a227e-117">If you are not using Eclipse to create this Java application, ensure the Twilio JAR is included within the same Azure role as your Java application, and added to the class path of your application.</span></span>
5. <span data-ttu-id="a227e-118">Ujistěte se, úložiště klíčů vaší cacerts obsahuje certifikát Equifax zabezpečení certifikační autority s MD5 otisk prstu 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (sériové číslo je 35:DE:F4:CF a otisk prstu SHA1 je D2:32:09:AD:23:D 3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A).</span><span class="sxs-lookup"><span data-stu-id="a227e-118">Ensure your cacerts keystore contains the Equifax Secure Certificate Authority certificate with MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (the serial number is 35:DE:F4:CF and the SHA1 fingerprint is D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A).</span></span> <span data-ttu-id="a227e-119">Toto je certifikát certifikační autority certifikátu pro [https://api.twilio.com] [ twilio_api_service] služba, která je volána, když používáte rozhraní API Twilio.</span><span class="sxs-lookup"><span data-stu-id="a227e-119">This is the certificate authority (CA) certificate for the [https://api.twilio.com][twilio_api_service] service, which is called when you use Twilio APIs.</span></span> <span data-ttu-id="a227e-120">Informace o přidání tento certifikát certifikační Autority vaší JDK cacert úložiště najdete v tématu [přidání certifikátu do úložiště certifikátů certifikační Autority Java][add_ca_cert].</span><span class="sxs-lookup"><span data-stu-id="a227e-120">For information about adding this CA certificate to your JDK's cacert store, see [Adding a Certificate to the Java CA Certificate Store][add_ca_cert].</span></span>

<span data-ttu-id="a227e-121">Kromě toho znalost na informace na [vytváření Hello World aplikace pomocí nástrojů Azure pro Eclipse][azure_java_eclipse_hello_world], nebo s jinými technikami pro hostování aplikací Java v Azure, pokud jste nepoužíváte Eclipse, důrazně doporučujeme.</span><span class="sxs-lookup"><span data-stu-id="a227e-121">Additionally, familiarity with the information at [Creating a Hello World Application Using the Azure Toolkit for Eclipse][azure_java_eclipse_hello_world], or with other techniques for hosting Java applications in Azure if you are not using Eclipse, is highly recommended.</span></span>

## <a name="create-a-web-form-for-making-a-call"></a><span data-ttu-id="a227e-122">Vytvoření webového formuláře pro volání</span><span class="sxs-lookup"><span data-stu-id="a227e-122">Create a web form for making a call</span></span>
<span data-ttu-id="a227e-123">Následující kód ukazuje, jak vytvořit webového formuláře pro načtení dat uživatele pro volání.</span><span class="sxs-lookup"><span data-stu-id="a227e-123">The following code shows how to create a web form to retrieve user data for making a call.</span></span> <span data-ttu-id="a227e-124">Pro účely tohoto příkladu nového dynamického webového projektu, s názvem **TwilioCloud**, byl vytvořen, a **callform.jsp** byl přidán jako soubor JSP.</span><span class="sxs-lookup"><span data-stu-id="a227e-124">For purposes of this example, a new dynamic web project, named **TwilioCloud**, was created, and **callform.jsp** was added as a JSP file.</span></span>

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
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
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

## <a name="create-the-code-to-make-the-call"></a><span data-ttu-id="a227e-125">Vytvoření kódu pro volání</span><span class="sxs-lookup"><span data-stu-id="a227e-125">Create the code to make the call</span></span>
<span data-ttu-id="a227e-126">Následující kód, který je volána, když uživatel dokončí formulář zobrazuje callform.jsp, vytvoří zprávu volání a generuje volání.</span><span class="sxs-lookup"><span data-stu-id="a227e-126">The following code, which is called when the user completes the form displayed by callform.jsp, creates the call message and generates the call.</span></span> <span data-ttu-id="a227e-127">Pro účely tohoto příkladu je název souboru JSP **makecall.jsp** a byl přidán do **TwilioCloud** projektu.</span><span class="sxs-lookup"><span data-stu-id="a227e-127">For purposes of this example, the JSP file is named **makecall.jsp** and was added to the **TwilioCloud** project.</span></span> <span data-ttu-id="a227e-128">(Použití vašeho účtu Twilio a ověřování tokenu místo zástupné hodnoty přiřazené **accountSID** a **ověřovacího tokenu** v následujícím kódu.)</span><span class="sxs-lookup"><span data-stu-id="a227e-128">(Use your Twilio account and authentication token instead of the placeholder values assigned to **accountSID** and **authToken** in the code below.)</span></span>

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
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";

         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");

         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");

         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");

         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");

         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;

         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");

         // Place the call From, To and URL values into a hash map. 
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

<span data-ttu-id="a227e-129">Kromě vytváření volání, zobrazí makecall.jsp Twilio koncový bod, verze rozhraní API a stav volání.</span><span class="sxs-lookup"><span data-stu-id="a227e-129">In addition to making the call, makecall.jsp displays the Twilio endpoint, API version, and the call status.</span></span> <span data-ttu-id="a227e-130">Na následujícím snímku obrazovky je například:</span><span class="sxs-lookup"><span data-stu-id="a227e-130">An example is the following screen shot:</span></span>

![Azure volání odpovědi pomocí Twilio a Java][twilio_java_response]

## <a name="run-the-application"></a><span data-ttu-id="a227e-132">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="a227e-132">Run the application</span></span>
<span data-ttu-id="a227e-133">Tady jsou hlavní kroky při spuštění aplikace; Podrobnosti o těchto krocích naleznete v [vytváření Hello World aplikace pomocí nástrojů Azure pro Eclipse][azure_java_eclipse_hello_world].</span><span class="sxs-lookup"><span data-stu-id="a227e-133">Following are the high-level steps to run your application; details for these steps can be found at [Creating a Hello World Application Using the Azure Toolkit for Eclipse][azure_java_eclipse_hello_world].</span></span>

1. <span data-ttu-id="a227e-134">Export vašeho TwilioCloud WAR do Azure **approot** složky.</span><span class="sxs-lookup"><span data-stu-id="a227e-134">Export your TwilioCloud WAR to the Azure **approot** folder.</span></span> 
2. <span data-ttu-id="a227e-135">Upravit **startup.cmd** dekomprimovat vaší TwilioCloud WAR.</span><span class="sxs-lookup"><span data-stu-id="a227e-135">Modify **startup.cmd** to unzip your TwilioCloud WAR.</span></span>
3. <span data-ttu-id="a227e-136">Zkompilujte aplikaci pro emulátoru služby výpočty v.</span><span class="sxs-lookup"><span data-stu-id="a227e-136">Compile your application for the compute emulator.</span></span>
4. <span data-ttu-id="a227e-137">Spusťte v emulátoru služby výpočty v nasazení.</span><span class="sxs-lookup"><span data-stu-id="a227e-137">Start your deployment in the compute emulator.</span></span>
5. <span data-ttu-id="a227e-138">Otevřete prohlížeč a spusťte **http://localhost:8080/TwilioCloud/callform.jsp**.</span><span class="sxs-lookup"><span data-stu-id="a227e-138">Open a browser, and run **http://localhost:8080/TwilioCloud/callform.jsp**.</span></span>
6. <span data-ttu-id="a227e-139">Zadejte hodnoty ve formátu, klikněte na tlačítko **toto volání**a pak zobrazte výsledky v makecall.jsp.</span><span class="sxs-lookup"><span data-stu-id="a227e-139">Enter values in the form, click **Make this call**, and then see the results in makecall.jsp.</span></span>

<span data-ttu-id="a227e-140">Až budete připraveni k nasazení do Azure, a pak ji znovu zkompilovat pro nasazení do cloudu a nasazení do Azure a spusťte http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp v prohlížeči (nahraďte hodnota pro  *your_hosted_name*).</span><span class="sxs-lookup"><span data-stu-id="a227e-140">When you are ready to deploy to Azure, recompile for deployment to the cloud, deploy to Azure, and run http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp in the browser (substitute your value for *your_hosted_name*).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a227e-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a227e-141">Next steps</span></span>
<span data-ttu-id="a227e-142">Tento kód byl poskytnut tak, aby zobrazovalo základních funkcí pomocí Twilio v jazyce Java v Azure.</span><span class="sxs-lookup"><span data-stu-id="a227e-142">This code was provided to show you basic functionality using Twilio in Java on Azure.</span></span> <span data-ttu-id="a227e-143">Před nasazením do Azure v produkčním prostředí, můžete přidat další zpracování chyb a další funkce.</span><span class="sxs-lookup"><span data-stu-id="a227e-143">Before deploying to Azure in production, you may want to add more error handling or other features.</span></span> <span data-ttu-id="a227e-144">Například:</span><span class="sxs-lookup"><span data-stu-id="a227e-144">For example:</span></span>

* <span data-ttu-id="a227e-145">Místo použití webového formuláře, můžete použít objekty BLOB úložiště Azure nebo databázi SQL pro ukládání telefonních čísel a volání text.</span><span class="sxs-lookup"><span data-stu-id="a227e-145">Instead of using a web form, you could use Azure storage blobs or SQL Database to store phone numbers and call text.</span></span> <span data-ttu-id="a227e-146">Informace o použití objektů BLOB služby Azure storage v jazyce Java najdete v tématu [jak používat služby úložiště objektů Blob z Javy][howto_blob_storage_java].</span><span class="sxs-lookup"><span data-stu-id="a227e-146">For information about using Azure storage blobs in Java, see [How to Use the Blob Storage Service from Java][howto_blob_storage_java].</span></span> <span data-ttu-id="a227e-147">Informace o používání databáze SQL v jazyce Java najdete v tématu [pomocí SQL Database v jazyce Java][howto_sql_azure_java].</span><span class="sxs-lookup"><span data-stu-id="a227e-147">For information about using SQL Database in Java, see [Using SQL Database in Java][howto_sql_azure_java].</span></span>
* <span data-ttu-id="a227e-148">Můžete použít **RoleEnvironment.getConfigurationSettings** načíst Twilio ID účtu a ověřování tokenu z nastavení konfigurace vašeho nasazení, místo pevně kódováno hodnoty v makecall.jsp.</span><span class="sxs-lookup"><span data-stu-id="a227e-148">You could use **RoleEnvironment.getConfigurationSettings** to retrieve the Twilio account ID and authentication token from your deployment's configuration settings, instead of hard-coding the values in makecall.jsp.</span></span> <span data-ttu-id="a227e-149">Informace o **RoleEnvironment** třídy najdete v tématu [pomocí běhové knihovny služby Azure v JSP] [ azure_runtime_jsp] a v dokumentaci k balíčku běh služby Azure na [http://dl.windowsazure.com/javadoc][azure_javadoc].</span><span class="sxs-lookup"><span data-stu-id="a227e-149">For information about the **RoleEnvironment** class, see [Using the Azure Service Runtime Library in JSP][azure_runtime_jsp] and the Azure Service Runtime package documentation at [http://dl.windowsazure.com/javadoc][azure_javadoc].</span></span>
* <span data-ttu-id="a227e-150">Kód makecall.jsp přiřadí URL poskytnutou Twilio, [http://twimlets.com/message][twimlet_message_url]do **Url** proměnné.</span><span class="sxs-lookup"><span data-stu-id="a227e-150">The makecall.jsp code assigns a Twilio-provided URL, [http://twimlets.com/message][twimlet_message_url], to the **Url** variable.</span></span> <span data-ttu-id="a227e-151">Tato adresa URL obsahuje odpovědi Twilio Markup Language (TwiML), která informuje Twilio postup při volání.</span><span class="sxs-lookup"><span data-stu-id="a227e-151">This URL provides a Twilio Markup Language (TwiML) response that informs Twilio how to proceed with the call.</span></span> <span data-ttu-id="a227e-152">Například může obsahovat TwiML, která je vrácena  **&lt;indikované&gt;**  akci, která je výsledkem text použitém k příjemce volání.</span><span class="sxs-lookup"><span data-stu-id="a227e-152">For example, the TwiML that is returned can contain a **&lt;Say&gt;** verb that results in text being spoken to the call recipient.</span></span> <span data-ttu-id="a227e-153">Místo použití URL poskytnutou Twilio, může vytvořit vlastní služba neodpoví na požadavek na Twilio; Další informace najdete v tématu [jak Twilio použijte pro hlasový a možnosti serveru SMS v jazyce Java][howto_twilio_voice_sms_java].</span><span class="sxs-lookup"><span data-stu-id="a227e-153">Instead of using the Twilio-provided URL, you could build your own service to respond to Twilio's request; for more information, see [How to Use Twilio for Voice and SMS Capabilities in Java][howto_twilio_voice_sms_java].</span></span> <span data-ttu-id="a227e-154">Další informace o TwiML lze najít na [http://www.twilio.com/docs/api/twiml][twiml]a další informace o  **&lt;indikované&gt;**  a ostatní operace Twilio naleznete na adrese [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="a227e-154">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml], and more information about **&lt;Say&gt;** and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>
* <span data-ttu-id="a227e-155">Přečtěte si pokyny zabezpečení Twilio v [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="a227e-155">Read the Twilio security guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>

<span data-ttu-id="a227e-156">Další informace o Twilio najdete v tématu [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="a227e-156">For additional information about Twilio, see [https://www.twilio.com/docs][twilio_docs].</span></span>

## <a name="see-also"></a><span data-ttu-id="a227e-157">Viz také</span><span class="sxs-lookup"><span data-stu-id="a227e-157">See Also</span></span>
* <span data-ttu-id="a227e-158">[Jak používat Twilio pro hlasový a možnosti serveru SMS v jazyce Java][howto_twilio_voice_sms_java]</span><span class="sxs-lookup"><span data-stu-id="a227e-158">[How to Use Twilio for Voice and SMS Capabilities in Java][howto_twilio_voice_sms_java]</span></span>
* <span data-ttu-id="a227e-159">[Přidání certifikátu do úložiště certifikátů certifikační Autority Java][add_ca_cert]</span><span class="sxs-lookup"><span data-stu-id="a227e-159">[Adding a Certificate to the Java CA Certificate Store][add_ca_cert]</span></span>

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
