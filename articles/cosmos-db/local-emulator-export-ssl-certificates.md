---
title: "Exportu certifikátů emulátoru DB Cosmos Azure | Microsoft Docs"
description: "Při vývoji v jazycích a moduly runtime, která nepoužívají Windows Store certifikát budete muset exportovat a spravovat certifikáty SSL. Tento příspěvek poskytuje podrobné pokyny."
services: cosmos-db
documentationcenter: 
keywords: "Emulátor Azure Cosmos DB"
author: voellm
manager: jhubbard
editor: 
ms.assetid: ef43deda-c2e9-4193-99e2-7f6a88a0319f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: tvoellm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4add5028d50972316902cecd8c399781c012cb77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="export-the-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a><span data-ttu-id="f0ddf-105">Exportu certifikátů emulátoru DB Cosmos Azure pro použití s Java, Python a Node.js</span><span class="sxs-lookup"><span data-stu-id="f0ddf-105">Export the Azure Cosmos DB Emulator certificates for use with Java, Python, and Node.js</span></span>

[<span data-ttu-id="f0ddf-106">**Stáhněte si v emulátoru**</span><span class="sxs-lookup"><span data-stu-id="f0ddf-106">**Download the Emulator**</span></span>](https://aka.ms/cosmosdb-emulator)

<span data-ttu-id="f0ddf-107">Emulátor DB Cosmos Azure poskytuje místní prostředí, které emuluje služby Azure Cosmos DB pro účely vývoje, včetně jeho použití připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-107">The Azure Cosmos DB Emulator provides a local environment that emulates the Azure Cosmos DB service for development purposes including its use of SSL connections.</span></span> <span data-ttu-id="f0ddf-108">Tento příspěvek ukazuje, jak k exportu certifikátů SSL pro použití v jazycích a moduly runtime, který nelze integrovat do úložiště certifikátů systému Windows, například Java, která používá vlastní [úložiště certifikátů](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) a Python, který používá [soketu obálky](https://docs.python.org/2/library/ssl.html) a Node.js, která používá [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span><span class="sxs-lookup"><span data-stu-id="f0ddf-108">This post demonstrates how to export the SSL certificates for use in languages and runtimes that do not integrate with the Windows Certificate Store such as Java which uses its own [certificate store](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) and Python which uses [socket wrappers](https://docs.python.org/2/library/ssl.html) and Node.js which uses [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span></span> <span data-ttu-id="f0ddf-109">Další informace o emulátoru v [použití emulátoru DB Cosmos Azure pro vývoj a testování](./local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="f0ddf-109">You can read more about the emulator in [Use the Azure Cosmos DB Emulator for development and testing](./local-emulator.md).</span></span>

<span data-ttu-id="f0ddf-110">Tento kurz obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="f0ddf-110">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f0ddf-111">Otáčení certifikáty</span><span class="sxs-lookup"><span data-stu-id="f0ddf-111">Rotating certificates</span></span>
> * <span data-ttu-id="f0ddf-112">Export certifikátu protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="f0ddf-112">Exporting SSL certificate</span></span>
> * <span data-ttu-id="f0ddf-113">Informace o způsobu použití certifikátu v jazyce Java, Python a Node.js</span><span class="sxs-lookup"><span data-stu-id="f0ddf-113">Learning how to use the certificate in Java, Python, and Node.js</span></span>

## <a name="certification-rotation"></a><span data-ttu-id="f0ddf-114">Otočení certifikační</span><span class="sxs-lookup"><span data-stu-id="f0ddf-114">Certification rotation</span></span>

<span data-ttu-id="f0ddf-115">Certifikáty v emulátoru místního DB Cosmos Azure jsou generovány při prvním spuštění emulátor.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-115">Certificates in the Azure Cosmos DB Local Emulator are generated the first time the emulator is run.</span></span> <span data-ttu-id="f0ddf-116">Existují dva certifikáty.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-116">There are two certificates.</span></span> <span data-ttu-id="f0ddf-117">Jeden použité pro připojování k místní emulátoru a jeden pro správu tajných klíčů v emulátoru.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-117">One used for connecting to the local emulator and one for managing secrets within the emulator.</span></span> <span data-ttu-id="f0ddf-118">Certifikát, který chcete exportovat, je připojení certifikát s popisným názvem "DocumentDBEmulatorCertificate".</span><span class="sxs-lookup"><span data-stu-id="f0ddf-118">The certificate you want to export is the connection certificate with the friendly name "DocumentDBEmulatorCertificate".</span></span>

<span data-ttu-id="f0ddf-119">Oba certifikáty je možné znovu generovat kliknutím **obnovit Data** jak je uvedeno níže z emulátoru DB Cosmos Azure je spuštěná na hlavním panelu Windows.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-119">Both certificates can be regenerated by clicking **Reset Data** as shown below from Azure Cosmos DB Emulator running in the Windows Tray.</span></span> <span data-ttu-id="f0ddf-120">Máte-li znovu vygenerovat certifikáty a byly nainstalovány do úložiště certifikátů Java nebo je použili jinde je nutné je aktualizovat, jinak aplikace se už připojí k místní emulátor.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-120">If you regenerate the certificates and have installed them into the Java certificate store or used them elsewhere you will need to update them, otherwise your application will no longer connect to the local emulator.</span></span>

![Azure Cosmos DB místní emulátoru obnovení dat](./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png)

## <a name="how-to-export-the-azure-cosmos-db-ssl-certificate"></a><span data-ttu-id="f0ddf-122">Tom, jak exportovat certifikát Azure Cosmos DB SSL</span><span class="sxs-lookup"><span data-stu-id="f0ddf-122">How to export the Azure Cosmos DB SSL certificate</span></span>

1. <span data-ttu-id="f0ddf-123">Spusťte správce certifikátů systému Windows ve spuštění certlm.msc a přejděte do složky-> osobní certifikáty a otevřete certifikát s popisným názvem **DocumentDbEmulatorCertificate**.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-123">Start the Windows Certificate manager by running certlm.msc and navigate to the Personal->Certificates folder and open the certificate with the friendly name **DocumentDbEmulatorCertificate**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 1](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png)

2. <span data-ttu-id="f0ddf-125">Klikněte na **podrobnosti** pak **OK**.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-125">Click on **Details** then **OK**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 2](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png)

3. <span data-ttu-id="f0ddf-127">Klikněte na tlačítko **kopírovat do souboru...** .</span><span class="sxs-lookup"><span data-stu-id="f0ddf-127">Click **Copy to File...**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 3](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png)

4. <span data-ttu-id="f0ddf-129">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-129">Click **Next**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 4](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png)

5. <span data-ttu-id="f0ddf-131">Klikněte na tlačítko **Ne, neexportovat privátní klíč**, pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-131">Click **No, do not export private key**, then click **Next**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 5](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png)

6. <span data-ttu-id="f0ddf-133">Klikněte na **X.509 s kódováním Base-64 (. CER)** a potom **Další**.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-133">Click on **Base-64 encoded X.509 (.CER)** and then **Next**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 6](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png)

7. <span data-ttu-id="f0ddf-135">Zadejte název certifikátu.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-135">Give the certificate a name.</span></span> <span data-ttu-id="f0ddf-136">V takovém případě **documentdbemulatorcert** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-136">In this case **documentdbemulatorcert** and then click **Next**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 7](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png)

8. <span data-ttu-id="f0ddf-138">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-138">Click **Finish**.</span></span>

    ![Krok 8 exportovat Azure Cosmos DB místní emulátoru](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png)

## <a name="how-to-use-the-certificate-in-java"></a><span data-ttu-id="f0ddf-140">Postup použití certifikátu v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="f0ddf-140">How to use the certificate in Java</span></span>

<span data-ttu-id="f0ddf-141">Při spuštění aplikací v jazyce Java nebo MongoDB aplikací, které používají klienta Java je snazší nainstalujte certifikát do úložiště certifikátů výchozí Java než předávání "-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword="<password>"příznaky.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-141">When running Java applications or MongoDB applications that use the Java client it is easier to install the certificate into the Java default certificate store than passing the "-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword="<password>" flags.</span></span> <span data-ttu-id="f0ddf-142">Například zahrnuty [Java ukázkové aplikace](https://localhost:8081/_explorer/index.html) závisí na výchozím úložišti certifikátů.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-142">For example the included [Java Demo application](https://localhost:8081/_explorer/index.html) depends on the default certificate store.</span></span>

<span data-ttu-id="f0ddf-143">Postupujte podle pokynů [přidání certifikátu do úložiště certifikátů certifikační Autority Java](https://docs.microsoft.com/azure/java-add-certificate-ca-store) pro import certifikátu X.509 do výchozího úložiště certifikátů Java.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-143">Follow the instructions in the [Adding a Certificate to the Java CA Certificates Store](https://docs.microsoft.com/azure/java-add-certificate-ca-store) to import the X.509 certificate into the default Java certificate store.</span></span> <span data-ttu-id="f0ddf-144">Uvědomte si, že pracovat v adresáři % JAVA_HOME % při spuštění příkazu keytool.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-144">Keep in mind you will be working in the %JAVA_HOME% directory when running keytool.</span></span>

<span data-ttu-id="f0ddf-145">Jednou "CosmosDBEmulatorCertificate" SSL je nainstalován certifikát pro aplikaci by mohli připojit a používat místní emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-145">Once the "CosmosDBEmulatorCertificate" SSL certificate is installed your application should be able to connect and use the local Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="f0ddf-146">Pokud budete pokračovat, potíže se můžete použít postup popsaný [ladění připojení protokolem SSL/TLS](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) článku.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-146">If you continue to have trouble you may want to follow the [Debugging SSL/TLS Connections](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) article.</span></span> <span data-ttu-id="f0ddf-147">Je velmi pravděpodobné že certifikát není nainstalovaný do %JAVA_HOME%/jre/lib/security/cacerts úložiště.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-147">It is very likely the certificate is not installed into the %JAVA_HOME%/jre/lib/security/cacerts store.</span></span> <span data-ttu-id="f0ddf-148">Pro příklad, pokud máte víc nainstalovaných verzí aplikace Java může pomocí různých cacerts úložiště než ten, který jste aktualizovali.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-148">For example if you have multiple installed versions of Java your application may be using a different cacerts store than the one you updated.</span></span>

## <a name="how-to-use-the-certificate-in-python"></a><span data-ttu-id="f0ddf-149">Postup použití certifikátu v Pythonu</span><span class="sxs-lookup"><span data-stu-id="f0ddf-149">How to use the certificate in Python</span></span>

<span data-ttu-id="f0ddf-150">Ve výchozím nastavení [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) pro rozhraní API DocumentDB nebude zkuste a použít certifikát SSL při připojování k místní emulátor.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-150">By default the [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) for the DocumentDB API will not try and use the SSL certificate when connecting to the local emulator.</span></span> <span data-ttu-id="f0ddf-151">Pokud ale chcete použít ověřování SSL můžete podle příklady v [Python soketu obálky](https://docs.python.org/2/library/ssl.html) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-151">If however you want to use SSL validation you can follow the examples in the [Python socket wrappers](https://docs.python.org/2/library/ssl.html) documentation.</span></span>

## <a name="how-to-use-the-certificate-in-nodejs"></a><span data-ttu-id="f0ddf-152">Použití certifikátu v Node.js</span><span class="sxs-lookup"><span data-stu-id="f0ddf-152">How to use the certificate in Node.js</span></span>

<span data-ttu-id="f0ddf-153">Ve výchozím nastavení [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) pro rozhraní API DocumentDB nebude zkuste a použít certifikát SSL při připojování k místní emulátor.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-153">By default the [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) for the DocumentDB API will not try and use the SSL certificate when connecting to the local emulator.</span></span> <span data-ttu-id="f0ddf-154">Pokud ale chcete použít ověřování SSL můžete podle příklady v [Node.js dokumentaci](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span><span class="sxs-lookup"><span data-stu-id="f0ddf-154">If however you want to use SSL validation you can follow the examples in the [Node.js documentation](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0ddf-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0ddf-155">Next steps</span></span>

<span data-ttu-id="f0ddf-156">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="f0ddf-156">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f0ddf-157">Otočený certifikáty</span><span class="sxs-lookup"><span data-stu-id="f0ddf-157">Rotated certificates</span></span>
> * <span data-ttu-id="f0ddf-158">Exportovat certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="f0ddf-158">Exported the SSL certificate</span></span>
> * <span data-ttu-id="f0ddf-159">Zjistili, jak pro použití certifikátu v jazyce Java, Python a Node.js</span><span class="sxs-lookup"><span data-stu-id="f0ddf-159">Learned how to use the certificate in Java, Python and Node.js</span></span>

<span data-ttu-id="f0ddf-160">Nyní můžete přejít k části koncepty pro další informace o Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f0ddf-160">You can now proceed to the Concepts section for more information about Cosmos DB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f0ddf-161">Globální distribuce</span><span class="sxs-lookup"><span data-stu-id="f0ddf-161">Global distribution</span></span>](distribute-data-globally.md) 
