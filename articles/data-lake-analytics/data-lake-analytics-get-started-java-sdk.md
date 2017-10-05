---
title: "Použití Data Lake Analytics Java SDK k vývoji aplikací | Microsoft Docs"
description: "Použití sady Java SDK Azure Data Lake Analytics k vývoji aplikací"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 07830b36-2fe3-4809-a846-129cf67b6a9e
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: 795d9ec0b0cac5d74673404f1d0d851393336df0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-java-sdk"></a><span data-ttu-id="10b54-103">Začínáme s Azure Data Lake Analytics s využitím sady Java SDK</span><span class="sxs-lookup"><span data-stu-id="10b54-103">Get started with Azure Data Lake Analytics using Java SDK</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="10b54-104">Naučte se používat Azure Data Lake Analytics Java SDK k vytvoření účtu Azure Data Lake a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu a pracovat s úlohami.</span><span class="sxs-lookup"><span data-stu-id="10b54-104">Learn how to use the Azure Data Lake Analytics Java SDK to create an Azure Data Lake account and perform basic operations such as create folders, upload and download data files, delete your account, and work with jobs.</span></span> <span data-ttu-id="10b54-105">Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="10b54-105">For more information about Data Lake, see [Azure Data Lake Analytics](data-lake-analytics-overview.md).</span></span>

<span data-ttu-id="10b54-106">V tomto kurzu budete vyvíjet konzolovou aplikaci Java, obsahující ukázky běžné úkoly správy a vytváření testovacích dat a odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="10b54-106">In this tutorial, you will develop a Java console application which contains samples of common administrative tasks as well as creating test data and submitting a job.</span></span>  <span data-ttu-id="10b54-107">Pokud chcete použít jiné podporované nástroje a absolvovat stejný kurz, klikněte na karty nahoře v této části.</span><span class="sxs-lookup"><span data-stu-id="10b54-107">To go through the same tutorial using other supported tools, click the tabs on the top of this section.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="10b54-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="10b54-108">Prerequisites</span></span>
* <span data-ttu-id="10b54-109">Java Development Kit (JDK) 8 (využívající jazyk Java verze 1.8).</span><span class="sxs-lookup"><span data-stu-id="10b54-109">Java Development Kit (JDK) 8 (using Java version 1.8).</span></span>
* <span data-ttu-id="10b54-110">IntelliJ nebo jiné vhodné vývojové prostředí Java.</span><span class="sxs-lookup"><span data-stu-id="10b54-110">IntelliJ or another suitable Java development environment.</span></span> <span data-ttu-id="10b54-111">Tato položka je nepovinná, ale doporučuje se.</span><span class="sxs-lookup"><span data-stu-id="10b54-111">This is optional but recommended.</span></span> <span data-ttu-id="10b54-112">Níže uvedené pokyny používají IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="10b54-112">The instructions below use IntelliJ.</span></span>
* <span data-ttu-id="10b54-113">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="10b54-113">**An Azure subscription**.</span></span> <span data-ttu-id="10b54-114">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="10b54-114">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="10b54-115">Vytvoření aplikace Azure Active Directory (AAD) a načtení **ID klienta**, **ID tenanta**, a **Klíče**.</span><span class="sxs-lookup"><span data-stu-id="10b54-115">Create an Azure Active Directory (AAD) application and retrieve its **Client ID**, **Tenant ID**, and **Key**.</span></span> <span data-ttu-id="10b54-116">Další informace o aplikacích AAD a pokyny k získání ID klienta naleznete v tématu [Vytvoření aplikace Active Directory a objektu služby pomocí portálu](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="10b54-116">For more information about AAD applications and instructions on how to get a client ID, see [Create Active Directory application and service principal using portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="10b54-117">Identifikátor URI odpovědi a klíč budou také k dispozici z portálu, jakmile vytvoříte aplikaci vygenerujete klíč.</span><span class="sxs-lookup"><span data-stu-id="10b54-117">The Reply URI and Key will also be available from the portal once you have the application created and key generated.</span></span>

## <a name="how-do-i-authenticate-using-azure-active-directory"></a><span data-ttu-id="10b54-118">Jak můžu ověřovat pomocí služby Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="10b54-118">How do I authenticate using Azure Active Directory?</span></span>
<span data-ttu-id="10b54-119">Následující fragment kódu obsahuje kód pro **neinteraktivní** ověřování, kdy aplikace poskytuje svoje vlastní přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="10b54-119">The code snippet below provides code for **non-interactive** authentication, where the application provides its own credentials.</span></span>

<span data-ttu-id="10b54-120">Pro účely tohoto kurzu je nutné, abyste aplikaci udělili oprávnění k vytváření prostředků v Azure.</span><span class="sxs-lookup"><span data-stu-id="10b54-120">You will need to give your application permission to create resources in Azure for this tutorial to work.</span></span> <span data-ttu-id="10b54-121">**Důrazně doporučujeme**, abyste této aplikaci pro účely tohoto kurzu udělili oprávnění Přispěvatel jenom k nové, nepoužité a prázdné skupině prostředků v předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="10b54-121">It is **highly recommended** that you only give this application Contributor permissions to a new, unused, and empty resource group in your Azure subscription for the purposes of this tutorial.</span></span>

## <a name="create-a-java-application"></a><span data-ttu-id="10b54-122">Vytvoření aplikace Java</span><span class="sxs-lookup"><span data-stu-id="10b54-122">Create a Java application</span></span>
1. <span data-ttu-id="10b54-123">Otevřete IntelliJ a pomocí šablony **aplikace příkazového řádku** vytvořte nový projekt v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="10b54-123">Open IntelliJ and create a new Java project using the **Command Line App** template.</span></span>
2. <span data-ttu-id="10b54-124">Klikněte pravým tlačítkem na projekt na levé straně obrazovky a klikněte na možnost **Přidat podporu architektury**.</span><span class="sxs-lookup"><span data-stu-id="10b54-124">Right-click on the project on the left-hand side of your screen and click **Add Framework Support**.</span></span> <span data-ttu-id="10b54-125">Vyberte možnost **Maven** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="10b54-125">Choose **Maven** and click **OK**.</span></span>
3. <span data-ttu-id="10b54-126">Otevřete nově vytvořený soubor **pom.xml** a mezi značky **\</version>** a **\</project>** přidejte následující fragment textu:</span><span class="sxs-lookup"><span data-stu-id="10b54-126">Open the newly created **"pom.xml"** file and add the following snippet of text between the **\</version>** tag and the **\</project>** tag:</span></span>

    >[!NOTE]
    ><span data-ttu-id="10b54-127">Tento krok je dočasný, dokud nebude k dispozici v nástroji Maven SDK služby Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="10b54-127">This step is temporary until the Azure Data Lake Analytics SDK is available in Maven.</span></span> <span data-ttu-id="10b54-128">Jakmile bude sada SDK dostupná v nástroji Maven, tento článek budeme aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="10b54-128">This article will be updated once the SDK is available in Maven.</span></span> <span data-ttu-id="10b54-129">Všechny budoucí aktualizace této sady SDK budou dostupné prostřednictvím nástroje Maven.</span><span class="sxs-lookup"><span data-stu-id="10b54-129">All future updates to this SDK will be availble through Maven.</span></span>
    >

        <repositories>
            <repository>
                <id>adx-snapshots</id>
                <name>Azure ADX Snapshots</name>
                <url>http://adxsnapshots.azurewebsites.net/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                </snapshots>
            </repository>
            <repository>
                <id>oss-snapshots</id>
                <name>Open Source Snapshots</name>
                <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                    <updatePolicy>always</updatePolicy>
                </snapshots>
            </repository>
        </repositories>
        <dependencies>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-client-authentication</artifactId>
                <version>1.0.0-20160513.000802-24</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-client-runtime</artifactId>
                <version>1.0.0-20160513.000812-28</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.rest</groupId>
                <artifactId>client-runtime</artifactId>
                <version>1.0.0-20160513.000825-29</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-mgmt-datalake-store</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-mgmt-datalake-analytics</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
        </dependencies>
4. <span data-ttu-id="10b54-130">Přejděte na **soubor**, pak **nastavení**, pak **sestavení**, **provádění**, **nasazení**.</span><span class="sxs-lookup"><span data-stu-id="10b54-130">Go to **File**, then **Settings**, then **Build**, **Execution**, **Deployment**.</span></span> <span data-ttu-id="10b54-131">Vyberte **nástroje sestavení**, **Maven**, **import**.</span><span class="sxs-lookup"><span data-stu-id="10b54-131">Select **Build Tools**, **Maven**, **Importing**.</span></span> <span data-ttu-id="10b54-132">Zkontrolujte **automaticky importovat projekty Maven**.</span><span class="sxs-lookup"><span data-stu-id="10b54-132">Then check **Import Maven projects automatically**.</span></span>
5. <span data-ttu-id="10b54-133">Otevřete **Main.java** a stávající blok kódu nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="10b54-133">Open **Main.java** and replace the existing code block with the following code.</span></span> <span data-ttu-id="10b54-134">Zadejte také hodnoty parametrů ve fragmentu kódu, například **localFolderPath**, **_adlaAccountName**, **_adlsAccountName**, **_ Název skupiny prostředků** a nahraďte zástupné symboly pro **CLIENT-ID**, **tajný klíč klienta**, **ID klienta**, a  **ID PŘEDPLATNÉHO**.</span><span class="sxs-lookup"><span data-stu-id="10b54-134">Also, provide the values for parameters called out in the code snippet, such as **localFolderPath**, **_adlaAccountName**, **_adlsAccountName**, **_resourceGroupName** and replace placeholders for **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID**, and **SUBSCRIPTION-ID**.</span></span>

    <span data-ttu-id="10b54-135">Tento kód projde procesem vytváření účtů Data Lake Store a Data Lake Analytics, vytvoření souborů v úložišti, spuštění úlohy, získávání stavu úlohy, výstup úlohy stahování a nakonec odstranění účtu.</span><span class="sxs-lookup"><span data-stu-id="10b54-135">This code goes through the process of creating Data Lake Store and Data Lake Analytics accounts, creating files in the store, running a job, getting job status, downloading job output, and finally deleting the account.</span></span>

   > [!NOTE]
   > <span data-ttu-id="10b54-136">Momentálně je známý problém se službou Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="10b54-136">There is currently a known issue with the Azure Data Lake Service.</span></span>  <span data-ttu-id="10b54-137">Pokud je vzorová aplikace přerušena nebo dojde k chybě, může být nutné ruční odstranění účtů Data Lake Store a Data Lake Analytics, které skript vytvoří.</span><span class="sxs-lookup"><span data-stu-id="10b54-137">If the sample app is interrupted or encounters an error, you may need to manually delete the Data Lake Store & Data Lake Analytics accounts that the script creates.</span></span>  <span data-ttu-id="10b54-138">Pokud nejste obeznámeni s portálem, začněte pomocí průvodce [Správa analýz Azure Data Lake Analytics pomocí portálu Azure](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="10b54-138">If you're not familiar with the Portal, the [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md) guide will get you started.</span></span>
   >
   >

        package com.company;

        import com.microsoft.azure.CloudException;
        import com.microsoft.azure.credentials.ApplicationTokenCredentials;
        import com.microsoft.azure.management.datalake.store.*;
        import com.microsoft.azure.management.datalake.store.models.*;
        import com.microsoft.azure.management.datalake.analytics.*;
        import com.microsoft.azure.management.datalake.analytics.models.*;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import java.io.*;
        import java.nio.charset.Charset;
        import java.nio.file.Files;
        import java.nio.file.Paths;
        import java.util.ArrayList;
        import java.util.UUID;
        import java.util.List;

        public class Main {
            private static String _adlsAccountName;
            private static String _adlaAccountName;
            private static String _resourceGroupName;
            private static String _location;

            private static String _tenantId;
            private static String _subId;
            private static String _clientId;
            private static String _clientSecret;

            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
            private static DataLakeAnalyticsAccountManagementClient _adlaClient;
            private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
            private static DataLakeAnalyticsCatalogManagementClient _adlaCatalogClient;

            public static void main(String[] args) throws Exception {
                _adlsAccountName = "<DATA-LAKE-STORE-NAME>";
                _adlaAccountName = "<DATA-LAKE-ANALYTICS-NAME>";
                _resourceGroupName = "<RESOURCE-GROUP-NAME>";
                _location = "East US 2";

                _tenantId = "<TENANT-ID>";
                _subId =  "<SUBSCRIPTION-ID>";
                _clientId = "<CLIENT-ID>";

                _clientSecret = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.

                String localFolderPath = "C:\\local_path\\"; // TODO: Change this to any unused, new, empty folder on your local machine.

                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
                SetupClients(creds);

                // Create Data Lake Store and Analytics accounts
                WaitForNewline("Authenticated.", "Creating NEW accounts.");
                CreateAccounts();
                WaitForNewline("Accounts created.", "Displaying accounts.");

                // List Data Lake Store and Analytics accounts that this app can access
                System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", _subId));
                List<DataLakeStoreAccount> adlsListResult = _adlsClient.getAccountOperations().list().getBody();
                for (DataLakeStoreAccount acct : adlsListResult) {
                    System.out.println(acct.getName());
                }
                System.out.println(String.format("All ADL Analytics accounts that this app can access in subscription %s:", _subId));
                List<DataLakeAnalyticsAccount> adlaListResult = _adlaClient.getAccountOperations().list().getBody();
                for (DataLakeAnalyticsAccount acct : adlaListResult) {
                    System.out.println(acct.getName());
                }
                WaitForNewline("Accounts displayed.", "Creating files.");

                // Create a file in Data Lake Store: input1.csv
                // TODO: these change order in the next patch
                byte[] bytesContents = "123,abc".getBytes();
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, "/input1.csv", bytesContents, true);

                WaitForNewline("File created.", "Submitting a job.");

                // Submit a job to Data Lake Analytics
                UUID jobId = SubmitJobByScript("@input =  EXTRACT Data string FROM \"/input1.csv\" USING Extractors.Csv(); OUTPUT @input TO @\"/output1.csv\" USING Outputters.Csv();", "testJob");
                WaitForNewline("Job submitted.", "Getting job status.");

                // Wait for job completion and output job status
                System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
                System.out.println("Waiting for job completion.");
                WaitForJob(jobId);
                System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
                WaitForNewline("Job completed.", "Downloading job output.");

                // Download job output from Data Lake Store
                DownloadFile("/output1.csv", localFolderPath + "output1.csv");
                WaitForNewline("Job output downloaded.", "Deleting file.");

                // Delete file from Data Lake Store
                DeleteFile("/output1.csv");
                WaitForNewline("File deleted.", "Deleting account.");

                // Delete account
                _adlsClient.getAccountOperations().delete(_resourceGroupName, _adlsAccountName);
                _adlaClient.getAccountOperations().delete(_resourceGroupName, _adlaAccountName);
                WaitForNewline("Account deleted.", "DONE.");
            }

            //Set up clients
            public static void SetupClients(ServiceClientCredentials creds)
            {
                _adlsClient = new DataLakeStoreAccountManagementClientImpl(creds);
                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClientImpl(creds);
                _adlaClient = new DataLakeAnalyticsAccountManagementClientImpl(creds);
                _adlaJobClient = new DataLakeAnalyticsJobManagementClientImpl(creds);
                _adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClientImpl(creds);
                _adlsClient.setSubscriptionId(_subId);
                _adlaClient.setSubscriptionId(_subId);
            }

            // Helper function to show status and wait for user input
            public static void WaitForNewline(String reason, String nextAction)
            {
                if (nextAction == null)
                    nextAction = "";

                System.out.println(reason + "\r\nPress ENTER to continue...");
                try{System.in.read();}
                catch(Exception e){}

                if (!nextAction.isEmpty())
                {
                    System.out.println(nextAction);
                }
            }

            // Create Data Lake Store and Analytics accounts
            public static void CreateAccounts() throws InterruptedException, CloudException, IOException {
                // Create ADLS account
                DataLakeStoreAccount adlsParameters = new DataLakeStoreAccount();
                adlsParameters.setLocation(_location);

                _adlsClient.getAccountOperations().create(_resourceGroupName, _adlsAccountName, adlsParameters);

                // Create ADLA account
                DataLakeStoreAccountInfo adlsInfo = new DataLakeStoreAccountInfo();
                adlsInfo.setName(_adlsAccountName);

                DataLakeStoreAccountInfoProperties adlsInfoProperties = new DataLakeStoreAccountInfoProperties();
                adlsInfo.setProperties(adlsInfoProperties);

                List<DataLakeStoreAccountInfo> adlsInfoList = new ArrayList<DataLakeStoreAccountInfo>();
                adlsInfoList.add(adlsInfo);

                DataLakeAnalyticsAccountProperties adlaProperties = new DataLakeAnalyticsAccountProperties();
                adlaProperties.setDataLakeStoreAccounts(adlsInfoList);
                adlaProperties.setDefaultDataLakeStoreAccount(_adlsAccountName);

                DataLakeAnalyticsAccount adlaParameters = new DataLakeAnalyticsAccount();
                adlaParameters.setLocation(_location);
                adlaParameters.setName(_adlaAccountName);
                adlaParameters.setProperties(adlaProperties);

                    /* If this line generates an error message like "The deep update for property 'DataLakeStoreAccounts' is not supported", please delete the ADLS and ADLA accounts via the portal and re-run your script. */

                _adlaClient.getAccountOperations().create(_resourceGroupName, _adlaAccountName, adlaParameters);
            }

            //todo: this changes in the next version of the API
            public static void CreateFile(String path, String contents, boolean force) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();

                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path, bytesContents, force);
            }

            public static void DeleteFile(String filePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().delete(filePath, _adlsAccountName);
            }

            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, CloudException {
                InputStream stream = _adlsFileSystemClient.getFileSystemOperations().open(srcPath, _adlsAccountName).getBody();

                PrintWriter pWriter = new PrintWriter(destPath, Charset.defaultCharset().name());

                String fileContents = "";
                if (stream != null) {
                    Writer writer = new StringWriter();

                    char[] buffer = new char[1024];
                    try {
                        Reader reader = new BufferedReader(
                                new InputStreamReader(stream, "UTF-8"));
                        int n;
                        while ((n = reader.read(buffer)) != -1) {
                            writer.write(buffer, 0, n);
                        }
                    } finally {
                        stream.close();
                    }
                    fileContents =  writer.toString();
                }

                pWriter.println(fileContents);
                pWriter.close();
            }

            // Submit a U-SQL job by providing script contents.
            // Returns the job ID
            public static UUID SubmitJobByScript(String script, String jobName) throws IOException, CloudException {
                UUID jobId = java.util.UUID.randomUUID();
                USqlJobProperties properties = new USqlJobProperties();
                properties.setScript(script);
                JobInformation parameters = new JobInformation();
                parameters.setName(jobName);
                parameters.setJobId(jobId);
                parameters.setType(JobType.USQL);
                parameters.setProperties(properties);

                JobInformation jobInfo = _adlaJobClient.getJobOperations().create(_adlaAccountName, jobId, parameters).getBody();

                return jobId;
            }

            // Wait for job completion
            public static JobResult WaitForJob(UUID jobId) throws IOException, CloudException {
                JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
                while (jobInfo.getState() != JobState.ENDED)
                {
                    jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName,jobId).getBody();
                }
                return jobInfo.getResult();
            }

            // Get job status
            public static String GetJobStatus(UUID jobId) throws IOException, CloudException {
                JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
                return jobInfo.getState().toValue();
            }
        }

1. <span data-ttu-id="10b54-139">Postupujte podle výzev a spusťte a dokončete aplikaci.</span><span class="sxs-lookup"><span data-stu-id="10b54-139">Follow the prompts to run and complete the application.</span></span>

## <a name="see-also"></a><span data-ttu-id="10b54-140">Viz také</span><span class="sxs-lookup"><span data-stu-id="10b54-140">See also</span></span>
* <span data-ttu-id="10b54-141">Pokud chcete použít jiné podporované nástroje a zobrazit stejný kurz, klikněte na selektory karet v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="10b54-141">To see the same tutorial using other tools, click the tab selectors on the top of the page.</span></span>
* <span data-ttu-id="10b54-142">Pokud chcete zobrazit komplexnější dotaz, přejděte k tématu [Analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="10b54-142">To see a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="10b54-143">Pokud chcete začít s vývojem aplikací U-SQL, přejděte k tématu [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="10b54-143">To get started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="10b54-144">Pokud se chcete naučit jazyk U-SQL, informace najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md) a [Referenční informace pro jazyk U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).</span><span class="sxs-lookup"><span data-stu-id="10b54-144">To learn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md), and [U-SQL language reference](http://go.microsoft.com/fwlink/?LinkId=691348).</span></span>
* <span data-ttu-id="10b54-145">Informace týkající se úloh správy najdete v tématu [Správa Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="10b54-145">For management tasks, see [Manage Azure Data Lake Analytics using Azure Portal](data-lake-analytics-manage-use-portal.md).</span></span>
* <span data-ttu-id="10b54-146">Přehled Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="10b54-146">To get an overview of Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>
