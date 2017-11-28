---
title: "certifikáty emulátoru DB Cosmos Azure hello aaaExport | Microsoft Docs"
description: "Při vývoji v jazycích a moduly runtime, která nepoužívají úložiště certifikátů Windows hello bude potřebovat tooexport a spravovat certifikáty SSL hello. Tento příspěvek poskytuje podrobné pokyny."
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
ms.openlocfilehash: db56cda856fccf93d71ae5b21c4090ccb9aa40a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="export-hello-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a><span data-ttu-id="d380f-105">Exportovat hello emulátoru DB Cosmos Azure certifikáty pro použití s Java, Python a Node.js</span><span class="sxs-lookup"><span data-stu-id="d380f-105">Export hello Azure Cosmos DB Emulator certificates for use with Java, Python, and Node.js</span></span>

[<span data-ttu-id="d380f-106">**Stáhnout hello emulátoru**</span><span class="sxs-lookup"><span data-stu-id="d380f-106">**Download hello Emulator**</span></span>](https://aka.ms/cosmosdb-emulator)

<span data-ttu-id="d380f-107">Hello emulátoru DB Cosmos Azure poskytuje místní prostředí, které emuluje hello služby Azure Cosmos DB pro účely vývoje, včetně jeho použití připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="d380f-107">hello Azure Cosmos DB Emulator provides a local environment that emulates hello Azure Cosmos DB service for development purposes including its use of SSL connections.</span></span> <span data-ttu-id="d380f-108">Tento příspěvek ukazuje, jak tooexport hello SSL certifikáty pro použití v jazycích a moduly runtime, který není integrovat hello úložiště certifikátů systému Windows, jako je například Java, která používá vlastní [úložiště certifikátů](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) a Python, který používá [soketu obálky](https://docs.python.org/2/library/ssl.html) a Node.js, která používá [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span><span class="sxs-lookup"><span data-stu-id="d380f-108">This post demonstrates how tooexport hello SSL certificates for use in languages and runtimes that do not integrate with hello Windows Certificate Store such as Java which uses its own [certificate store](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) and Python which uses [socket wrappers](https://docs.python.org/2/library/ssl.html) and Node.js which uses [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span></span> <span data-ttu-id="d380f-109">Další informace o emulátoru hello v [hello použití emulátoru DB Cosmos Azure pro vývoj a testování](./local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="d380f-109">You can read more about hello emulator in [Use hello Azure Cosmos DB Emulator for development and testing](./local-emulator.md).</span></span>

<span data-ttu-id="d380f-110">Tento kurz se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="d380f-110">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d380f-111">Otáčení certifikáty</span><span class="sxs-lookup"><span data-stu-id="d380f-111">Rotating certificates</span></span>
> * <span data-ttu-id="d380f-112">Export certifikátu protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="d380f-112">Exporting SSL certificate</span></span>
> * <span data-ttu-id="d380f-113">Učení jak toouse hello certifikátu v jazyce Java, Python a Node.js</span><span class="sxs-lookup"><span data-stu-id="d380f-113">Learning how toouse hello certificate in Java, Python, and Node.js</span></span>

## <a name="certification-rotation"></a><span data-ttu-id="d380f-114">Otočení certifikační</span><span class="sxs-lookup"><span data-stu-id="d380f-114">Certification rotation</span></span>

<span data-ttu-id="d380f-115">Certifikáty v hello místní emulátoru DB Cosmos Azure jsou generovány hello prvním spuštění hello emulátor.</span><span class="sxs-lookup"><span data-stu-id="d380f-115">Certificates in hello Azure Cosmos DB Local Emulator are generated hello first time hello emulator is run.</span></span> <span data-ttu-id="d380f-116">Existují dva certifikáty.</span><span class="sxs-lookup"><span data-stu-id="d380f-116">There are two certificates.</span></span> <span data-ttu-id="d380f-117">Jeden použité pro připojování toohello místní emulátoru a jeden pro správu tajných klíčů v emulátoru hello.</span><span class="sxs-lookup"><span data-stu-id="d380f-117">One used for connecting toohello local emulator and one for managing secrets within hello emulator.</span></span> <span data-ttu-id="d380f-118">Hello certifikátu, že který má tooexport je hello připojení certifikát s popisným názvem hello "DocumentDBEmulatorCertificate".</span><span class="sxs-lookup"><span data-stu-id="d380f-118">hello certificate you want tooexport is hello connection certificate with hello friendly name "DocumentDBEmulatorCertificate".</span></span>

<span data-ttu-id="d380f-119">Oba certifikáty je možné znovu generovat kliknutím **obnovit Data** jak je uvedeno níže z emulátoru DB Cosmos Azure spuštěné v panelu Windows hello.</span><span class="sxs-lookup"><span data-stu-id="d380f-119">Both certificates can be regenerated by clicking **Reset Data** as shown below from Azure Cosmos DB Emulator running in hello Windows Tray.</span></span> <span data-ttu-id="d380f-120">Máte-li znovu vygenerovat certifikáty hello a byly nainstalovány do úložiště certifikátů hello Java nebo je někde použili budete potřebovat tooupdate je, jinak aplikace už připojit místní emulátoru toohello.</span><span class="sxs-lookup"><span data-stu-id="d380f-120">If you regenerate hello certificates and have installed them into hello Java certificate store or used them elsewhere you will need tooupdate them, otherwise your application will no longer connect toohello local emulator.</span></span>

![Azure Cosmos DB místní emulátoru obnovení dat](./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png)

## <a name="how-tooexport-hello-azure-cosmos-db-ssl-certificate"></a><span data-ttu-id="d380f-122">Jak tooexport hello certifikát Azure Cosmos DB SSL</span><span class="sxs-lookup"><span data-stu-id="d380f-122">How tooexport hello Azure Cosmos DB SSL certificate</span></span>

1. <span data-ttu-id="d380f-123">Spusťte správce certifikátů Windows hello spuštěním certlm.msc a přejděte toohello osobní -> složky certifikáty a otevřete hello certifikát s popisným názvem hello **DocumentDbEmulatorCertificate**.</span><span class="sxs-lookup"><span data-stu-id="d380f-123">Start hello Windows Certificate manager by running certlm.msc and navigate toohello Personal->Certificates folder and open hello certificate with hello friendly name **DocumentDbEmulatorCertificate**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 1](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png)

2. <span data-ttu-id="d380f-125">Klikněte na **podrobnosti** pak **OK**.</span><span class="sxs-lookup"><span data-stu-id="d380f-125">Click on **Details** then **OK**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 2](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png)

3. <span data-ttu-id="d380f-127">Klikněte na tlačítko **zkopírujte tooFile...** .</span><span class="sxs-lookup"><span data-stu-id="d380f-127">Click **Copy tooFile...**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 3](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png)

4. <span data-ttu-id="d380f-129">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d380f-129">Click **Next**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 4](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png)

5. <span data-ttu-id="d380f-131">Klikněte na tlačítko **Ne, neexportovat privátní klíč**, pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="d380f-131">Click **No, do not export private key**, then click **Next**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 5](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png)

6. <span data-ttu-id="d380f-133">Klikněte na **X.509 s kódováním Base-64 (. CER)** a potom **Další**.</span><span class="sxs-lookup"><span data-stu-id="d380f-133">Click on **Base-64 encoded X.509 (.CER)** and then **Next**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 6](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png)

7. <span data-ttu-id="d380f-135">Zadejte název certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="d380f-135">Give hello certificate a name.</span></span> <span data-ttu-id="d380f-136">V takovém případě **documentdbemulatorcert** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d380f-136">In this case **documentdbemulatorcert** and then click **Next**.</span></span>

    ![Azure Cosmos DB místní emulátoru exportovat krok 7](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png)

8. <span data-ttu-id="d380f-138">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="d380f-138">Click **Finish**.</span></span>

    ![Krok 8 exportovat Azure Cosmos DB místní emulátoru](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png)

## <a name="how-toouse-hello-certificate-in-java"></a><span data-ttu-id="d380f-140">Jak toouse hello certifikátu v jazyce Java</span><span class="sxs-lookup"><span data-stu-id="d380f-140">How toouse hello certificate in Java</span></span>

<span data-ttu-id="d380f-141">Při spuštění aplikací v jazyce Java nebo MongoDB aplikací, které používají klienta Java hello je snazší certifikát tooinstall hello do úložiště certifikátů výchozí Java hello než předávání hello "-Djavax.net.ssl.trustStore=<keystore> - Djavax.net.ssl.trustStorePassword= "<password>" příznaky.</span><span class="sxs-lookup"><span data-stu-id="d380f-141">When running Java applications or MongoDB applications that use hello Java client it is easier tooinstall hello certificate into hello Java default certificate store than passing hello "-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword="<password>" flags.</span></span> <span data-ttu-id="d380f-142">Například hello zahrnuté [Java ukázkové aplikace](https://localhost:8081/_explorer/index.html) závisí na výchozím úložišti certifikátů hello.</span><span class="sxs-lookup"><span data-stu-id="d380f-142">For example hello included [Java Demo application](https://localhost:8081/_explorer/index.html) depends on hello default certificate store.</span></span>

<span data-ttu-id="d380f-143">Postupujte podle pokynů hello v hello [přidání certifikátu toohello, úložiště certifikátů certifikační Autority Java](https://docs.microsoft.com/azure/java-add-certificate-ca-store) tooimport hello X.509 certifikátu do úložiště certifikátů Java výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="d380f-143">Follow hello instructions in hello [Adding a Certificate toohello Java CA Certificates Store](https://docs.microsoft.com/azure/java-add-certificate-ca-store) tooimport hello X.509 certificate into hello default Java certificate store.</span></span> <span data-ttu-id="d380f-144">Zachovat v si, že pracovat v adresáři % JAVA_HOME % hello při spuštění příkazu keytool.</span><span class="sxs-lookup"><span data-stu-id="d380f-144">Keep in mind you will be working in hello %JAVA_HOME% directory when running keytool.</span></span>

<span data-ttu-id="d380f-145">Jednou hello "CosmosDBEmulatorCertificate" SSL je nainstalován certifikát pro vaše aplikace by měl být schopný tooconnect a použití hello místní emulátoru DB Cosmos Azure.</span><span class="sxs-lookup"><span data-stu-id="d380f-145">Once hello "CosmosDBEmulatorCertificate" SSL certificate is installed your application should be able tooconnect and use hello local Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="d380f-146">Pokud budete pokračovat toohave řešení problémů může být vhodné toofollow hello [ladění připojení protokolem SSL/TLS](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) článku.</span><span class="sxs-lookup"><span data-stu-id="d380f-146">If you continue toohave trouble you may want toofollow hello [Debugging SSL/TLS Connections](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) article.</span></span> <span data-ttu-id="d380f-147">Je velmi pravděpodobné hello certifikát není nainstalován do úložiště %JAVA_HOME%/jre/lib/security/cacerts hello.</span><span class="sxs-lookup"><span data-stu-id="d380f-147">It is very likely hello certificate is not installed into hello %JAVA_HOME%/jre/lib/security/cacerts store.</span></span> <span data-ttu-id="d380f-148">Pro příklad, pokud máte víc nainstalovaných verzí aplikace Java může pomocí různých cacerts úložiště, než jeden, který jste aktualizovali hello.</span><span class="sxs-lookup"><span data-stu-id="d380f-148">For example if you have multiple installed versions of Java your application may be using a different cacerts store than hello one you updated.</span></span>

## <a name="how-toouse-hello-certificate-in-python"></a><span data-ttu-id="d380f-149">Jak toouse hello certifikátu v Pythonu</span><span class="sxs-lookup"><span data-stu-id="d380f-149">How toouse hello certificate in Python</span></span>

<span data-ttu-id="d380f-150">Ve výchozím nastavení hello [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) pro hello nebude DocumentDB API zkuste a používat certifikát SSL hello při připojování toohello místní emulátor.</span><span class="sxs-lookup"><span data-stu-id="d380f-150">By default hello [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) for hello DocumentDB API will not try and use hello SSL certificate when connecting toohello local emulator.</span></span> <span data-ttu-id="d380f-151">Pokud však chcete, aby ověření toouse SSL můžete podle hello příklady v hello [Python soketu obálky](https://docs.python.org/2/library/ssl.html) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="d380f-151">If however you want toouse SSL validation you can follow hello examples in hello [Python socket wrappers](https://docs.python.org/2/library/ssl.html) documentation.</span></span>

## <a name="how-toouse-hello-certificate-in-nodejs"></a><span data-ttu-id="d380f-152">Jak toouse hello certifikátu v Node.js</span><span class="sxs-lookup"><span data-stu-id="d380f-152">How toouse hello certificate in Node.js</span></span>

<span data-ttu-id="d380f-153">Ve výchozím nastavení hello [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) pro hello nebude DocumentDB API zkuste a používat certifikát SSL hello při připojování toohello místní emulátor.</span><span class="sxs-lookup"><span data-stu-id="d380f-153">By default hello [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) for hello DocumentDB API will not try and use hello SSL certificate when connecting toohello local emulator.</span></span> <span data-ttu-id="d380f-154">Pokud však chcete, aby ověření toouse SSL můžete podle hello příklady v hello [Node.js dokumentaci](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span><span class="sxs-lookup"><span data-stu-id="d380f-154">If however you want toouse SSL validation you can follow hello examples in hello [Node.js documentation](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d380f-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d380f-155">Next steps</span></span>

<span data-ttu-id="d380f-156">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="d380f-156">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d380f-157">Otočený certifikáty</span><span class="sxs-lookup"><span data-stu-id="d380f-157">Rotated certificates</span></span>
> * <span data-ttu-id="d380f-158">Certifikát SSL exportovaný hello</span><span class="sxs-lookup"><span data-stu-id="d380f-158">Exported hello SSL certificate</span></span>
> * <span data-ttu-id="d380f-159">Naučili, jak toouse hello certifikátu v jazyce Java, Python a Node.js</span><span class="sxs-lookup"><span data-stu-id="d380f-159">Learned how toouse hello certificate in Java, Python and Node.js</span></span>

<span data-ttu-id="d380f-160">Nyní můžete přejít toohello koncepty části Další informace o Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d380f-160">You can now proceed toohello Concepts section for more information about Cosmos DB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d380f-161">Globální distribuce</span><span class="sxs-lookup"><span data-stu-id="d380f-161">Global distribution</span></span>](distribute-data-globally.md) 
