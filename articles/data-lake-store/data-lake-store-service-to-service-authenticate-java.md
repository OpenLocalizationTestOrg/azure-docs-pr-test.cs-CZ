---
title: "Ověřování služby služby: Java s Data Lake Store pomocí Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak dosáhnout service-to-service ověřování s Data Lake Store pomocí Azure Active Directory s Javou"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: e537d8a6ea53bf4366168727de8ef95b96281d5b
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="service-to-service-authentication-with-data-lake-store-using-java"></a>Service-to-service ověřování s Data Lake Store pomocí jazyka Java
> [!div class="op_single_selector"]
> * [Pomocí Javy](data-lake-store-service-to-service-authenticate-java.md)
> * [Pomocí sady .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Pomocí Pythonu](data-lake-store-service-to-service-authenticate-python.md)
> * [Pomocí rozhraní REST API](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

V tomto článku se dozvíte o použití sady Java SDK udělat service-to-service ověřování s Azure Data Lake Store. Ověření koncových uživatelů s Data Lake Store pomocí sady Java SDK není podporováno.

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Vytvoření aplikace Azure Active Directory "Web"**. Musí mít dokončili postup v [Service-to-service ověřování s Data Lake Store pomocí Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md).

* [Maven](https://maven.apache.org/install.html). V tomto kurzu se používá Maven pro závislosti sestavení a projektu. I když je možné sestavení vytvářet bez použití systému pro sestavení, jako je Maven a Gradle, tyto systémy podstatně usnadňují správu závislostí.

* (Volitelné) Rozhraní IDE, jako je například [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) nebo [Eclipse](https://www.eclipse.org/downloads/) nebo podobné.

## <a name="service-to-service-authentication"></a>Ověřování služba-služba
1. Vytvořte projekt Maven pomocí příkazu [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) z příkazového řádku nebo pomocí integrovaného vývojového rozhraní (IDE). Pokyny k vytvoření projektu jazyka Java s použitím IntelliJ najdete [zde](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Pokyny k vytvoření projektu s použitím Eclipse najdete [zde](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm).

2. Přidejte k souboru Maven **pom.xml** následující závislosti. Před značku **\</project>** přidejte následující fragment kódu:
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.2.3</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    První závislostí je použití sady SDK pro Data Lake Store (`azure-data-lake-store-sdk`) z úložiště maven. Druhou závislostí je zadání protokolovacího rozhraní (`slf4j-nop`), které se pro tuto aplikaci použije. Sada SDK pro službu Data Lake Store používá při protokolování [slf4j](http://www.slf4j.org/), takže máte možnost si vybrat z řady oblíbených protokolovacích rozhraní, jako je log4j, Java, logback atd., nebo nemusíte použít žádné protokolování. Pro tento příklad zakážeme protokolování a použijeme tedy vazbu **slf4j-nop**. Pokud chcete ve své aplikaci použít jiné možnosti protokolování, přečtěte si informace [zde](http://www.slf4j.org/manual.html#projectDep).

3. Přidejte do své aplikace následující příkazy pro import.

        import com.microsoft.azure.datalake.store.ADLException;
        import com.microsoft.azure.datalake.store.ADLStoreClient;
        import com.microsoft.azure.datalake.store.DirectoryEntry;
        import com.microsoft.azure.datalake.store.IfExists;
        import com.microsoft.azure.datalake.store.oauth2.AccessTokenProvider;
        import com.microsoft.azure.datalake.store.oauth2.ClientCredsTokenProvider;

4. Použít následující fragment kódu v aplikaci Java se získat token pro Active Directory webovou aplikaci jste vytvořili dříve pomocí jedné z podřízené třídy `AccessTokenProvider` (následující příklad používá `ClientCredsTokenProvider`). Poskytovatel tokenu má uložené přihlašovací údaje v mezipaměti a automaticky token obnovuje, pokud má vypršet jeho platnost. Je možné vytvořit vlastní měly podtřídy `AccessTokenProvider` tak tokeny jsou získány kódem vašeho zákazníka. Prozatím se právě použijeme zadanému v sadě SDK.

    Místo **FILL-IN-HERE** zadejte skutečné hodnoty pro webovou aplikaci Azure Active Directory.

        private static String clientId = "FILL-IN-HERE";
        private static String authTokenEndpoint = "FILL-IN-HERE";
        private static String clientKey = "FILL-IN-HERE";
    
        AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);   

V sadě SDK pro Data Lake Store jsou vhodné metody, které umožňují spravovat tokeny zabezpečení potřebné ke komunikaci s účtem služby Data Lake Store. Není ale povinné použít tuto sadu SDK a tyto metody. Můžete použít také jakýkoliv jiný způsob získání tokenu, například sadu [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java) nebo vlastní kód.

## <a name="next-steps"></a>Další postup
V tomto článku jste zjistili, jak používat ověřování koncových uživatelů k ověřování s Azure Data Lake Store pomocí sady Java SDK. Teď můžete prohlédnout v následujících článcích, kteří komunikují o použití sady Java SDK pro práci s Azure Data Lake Store.

* [Operace dat v Data Lake Store pomocí sady Java SDK](data-lake-store-get-started-java-sdk.md)


