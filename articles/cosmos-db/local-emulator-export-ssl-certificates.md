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
# <a name="export-hello-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a>Exportovat hello emulátoru DB Cosmos Azure certifikáty pro použití s Java, Python a Node.js

[**Stáhnout hello emulátoru**](https://aka.ms/cosmosdb-emulator)

Hello emulátoru DB Cosmos Azure poskytuje místní prostředí, které emuluje hello služby Azure Cosmos DB pro účely vývoje, včetně jeho použití připojení SSL. Tento příspěvek ukazuje, jak tooexport hello SSL certifikáty pro použití v jazycích a moduly runtime, který není integrovat hello úložiště certifikátů systému Windows, jako je například Java, která používá vlastní [úložiště certifikátů](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) a Python, který používá [soketu obálky](https://docs.python.org/2/library/ssl.html) a Node.js, která používá [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback). Další informace o emulátoru hello v [hello použití emulátoru DB Cosmos Azure pro vývoj a testování](./local-emulator.md).

Tento kurz se zabývá hello následující úlohy:

> [!div class="checklist"]
> * Otáčení certifikáty
> * Export certifikátu protokolu SSL
> * Učení jak toouse hello certifikátu v jazyce Java, Python a Node.js

## <a name="certification-rotation"></a>Otočení certifikační

Certifikáty v hello místní emulátoru DB Cosmos Azure jsou generovány hello prvním spuštění hello emulátor. Existují dva certifikáty. Jeden použité pro připojování toohello místní emulátoru a jeden pro správu tajných klíčů v emulátoru hello. Hello certifikátu, že který má tooexport je hello připojení certifikát s popisným názvem hello "DocumentDBEmulatorCertificate".

Oba certifikáty je možné znovu generovat kliknutím **obnovit Data** jak je uvedeno níže z emulátoru DB Cosmos Azure spuštěné v panelu Windows hello. Máte-li znovu vygenerovat certifikáty hello a byly nainstalovány do úložiště certifikátů hello Java nebo je někde použili budete potřebovat tooupdate je, jinak aplikace už připojit místní emulátoru toohello.

![Azure Cosmos DB místní emulátoru obnovení dat](./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png)

## <a name="how-tooexport-hello-azure-cosmos-db-ssl-certificate"></a>Jak tooexport hello certifikát Azure Cosmos DB SSL

1. Spusťte správce certifikátů Windows hello spuštěním certlm.msc a přejděte toohello osobní -> složky certifikáty a otevřete hello certifikát s popisným názvem hello **DocumentDbEmulatorCertificate**.

    ![Azure Cosmos DB místní emulátoru exportovat krok 1](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png)

2. Klikněte na **podrobnosti** pak **OK**.

    ![Azure Cosmos DB místní emulátoru exportovat krok 2](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png)

3. Klikněte na tlačítko **zkopírujte tooFile...** .

    ![Azure Cosmos DB místní emulátoru exportovat krok 3](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png)

4. Klikněte na **Další**.

    ![Azure Cosmos DB místní emulátoru exportovat krok 4](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png)

5. Klikněte na tlačítko **Ne, neexportovat privátní klíč**, pak klikněte na tlačítko **Další**.

    ![Azure Cosmos DB místní emulátoru exportovat krok 5](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png)

6. Klikněte na **X.509 s kódováním Base-64 (. CER)** a potom **Další**.

    ![Azure Cosmos DB místní emulátoru exportovat krok 6](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png)

7. Zadejte název certifikátu hello. V takovém případě **documentdbemulatorcert** a pak klikněte na **Další**.

    ![Azure Cosmos DB místní emulátoru exportovat krok 7](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png)

8. Klikněte na **Dokončit**.

    ![Krok 8 exportovat Azure Cosmos DB místní emulátoru](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png)

## <a name="how-toouse-hello-certificate-in-java"></a>Jak toouse hello certifikátu v jazyce Java

Při spuštění aplikací v jazyce Java nebo MongoDB aplikací, které používají klienta Java hello je snazší certifikát tooinstall hello do úložiště certifikátů výchozí Java hello než předávání hello "-Djavax.net.ssl.trustStore=<keystore> - Djavax.net.ssl.trustStorePassword= "<password>" příznaky. Například hello zahrnuté [Java ukázkové aplikace](https://localhost:8081/_explorer/index.html) závisí na výchozím úložišti certifikátů hello.

Postupujte podle pokynů hello v hello [přidání certifikátu toohello, úložiště certifikátů certifikační Autority Java](https://docs.microsoft.com/azure/java-add-certificate-ca-store) tooimport hello X.509 certifikátu do úložiště certifikátů Java výchozí hello. Zachovat v si, že pracovat v adresáři % JAVA_HOME % hello při spuštění příkazu keytool.

Jednou hello "CosmosDBEmulatorCertificate" SSL je nainstalován certifikát pro vaše aplikace by měl být schopný tooconnect a použití hello místní emulátoru DB Cosmos Azure. Pokud budete pokračovat toohave řešení problémů může být vhodné toofollow hello [ladění připojení protokolem SSL/TLS](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) článku. Je velmi pravděpodobné hello certifikát není nainstalován do úložiště %JAVA_HOME%/jre/lib/security/cacerts hello. Pro příklad, pokud máte víc nainstalovaných verzí aplikace Java může pomocí různých cacerts úložiště, než jeden, který jste aktualizovali hello.

## <a name="how-toouse-hello-certificate-in-python"></a>Jak toouse hello certifikátu v Pythonu

Ve výchozím nastavení hello [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) pro hello nebude DocumentDB API zkuste a používat certifikát SSL hello při připojování toohello místní emulátor. Pokud však chcete, aby ověření toouse SSL můžete podle hello příklady v hello [Python soketu obálky](https://docs.python.org/2/library/ssl.html) dokumentaci.

## <a name="how-toouse-hello-certificate-in-nodejs"></a>Jak toouse hello certifikátu v Node.js

Ve výchozím nastavení hello [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) pro hello nebude DocumentDB API zkuste a používat certifikát SSL hello při připojování toohello místní emulátor. Pokud však chcete, aby ověření toouse SSL můžete podle hello příklady v hello [Node.js dokumentaci](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).

## <a name="next-steps"></a>Další kroky

V tomto kurzu provedete krok hello následující:

> [!div class="checklist"]
> * Otočený certifikáty
> * Certifikát SSL exportovaný hello
> * Naučili, jak toouse hello certifikátu v jazyce Java, Python a Node.js

Nyní můžete přejít toohello koncepty části Další informace o Cosmos DB.

> [!div class="nextstepaction"]
> [Globální distribuce](distribute-data-globally.md) 
