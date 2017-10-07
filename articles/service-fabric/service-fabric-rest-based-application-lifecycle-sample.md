---
title: "Ukázka životního cyklu aplikace založené na aaaREST | Microsoft Docs"
description: "Microsoft Azure Service Fabric vzorku, který ukazuje životního cyklu aplikace hello pomocí rozhraní Service Fabric REST hello."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 0a374e53-ff23-4ee8-8cc6-259d41e118e7
ms.service: service-fabric
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/2/2016
ms.author: ryanwi
redirect_url: /rest/api/servicefabric/
ms.openlocfilehash: a6817edb932b3e9fc987dc7d90bcbb3c5eb91e64
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="rest-based-application-lifecycle-sample"></a>Ukázka životního cyklu aplikace založené na REST
Tento příklad znázorňuje životního cyklu aplikace Service Fabric hello prostřednictvím volání rozhraní REST API. Další informace o životním cyklu aplikací Service Fabric hello najdete v tématu [životního cyklu aplikace Service Fabric](service-fabric-application-lifecycle.md).

Tato ukázka provádí hello následující:

* Zřizuje hello **WordCount 1.0.0** ukázku z balíčku aplikace WordCount hello v úložišti bitové kopie hello.
* Zobrazí hello seznam typů aplikací, včetně WordCount 1.0.0.
* Vytvoří aplikaci WordCount hello jako **fabric: / WordCount**.
* Zobrazí hello seznam aplikací, včetně fabric: / WordCount verze 1.0.0.
* Verze hello 1.1.0 zřizuje hello WordCount ukázky z hello **WordCountUpgrade** balíčku aplikace v úložišti bitové kopie hello.
* Zobrazí hello seznamu typy aplikací, který zahrnuje obě WordCount 1.0.0 a **WordCount 1.1.0**.
* Upgraduje tooversion aplikace WordCount hello 1.1.0.
* Zobrazí hello seznam aplikací, které zahrnuje WordCount verze 1.1.0, ale už obsahuje WordCount verze 1.0.0.
* Odstraní aplikaci WordCount hello.
* Zobrazí hello seznam aplikací, už včetně fabric: / WordCount.
* Zruší podporu hello 1.1.0 verzi ukázka WordCount hello.
* Zobrazí seznam hello typy aplikací, které zahrnuje WordCount 1.0.0, ale už obsahuje WordCount 1.1.0.
* Zruší podporu hello 1.0.0 verzi ukázka WordCount hello.
* Zobrazí hello seznamu typy aplikací, který už zahrnuje WordCount.

## <a name="prerequisites"></a>Požadavky
Tato ukázka používá hello [WordCount ukázka](http://aka.ms/servicefabricsamples) (najít v hello **Začínáme** ukázky). Ukázka WordCount Hello musí být nejprve sestaven, a pak dva balíčky pro aplikace musí být zkopírovaný toohello úložiště bitových kopií.

| Složka | Popis |
| --- | --- |
| WordCount |Hello ukázkovou aplikaci WordCount. Hello **ApplicationManifest.xml** soubor obsahuje **ApplicationTypeVersion = "1.0.0"**. |
| WordCountUpgrade |Hello ukázkovou aplikaci WordCount. soubor ApplicationManifest.xml Hello je třeba změnit příliš**ApplicationTypeVersion = "1.1.0"** upgradu toooccur aplikace tooallow hello. |

toocreate hello balíčky aplikací a zkopírujte je úložiště image store toohello, proveďte následující kroky hello:

1. Kopírování **C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug** příliš**C:\Temp\WordCount**. Tím se vytvoří balíček aplikace WordCount hello.
2. Zkopírujte C:\Temp\WordCount příliš**C:\Temp\WordCountUpgrade**. Tím se vytvoří hello **WordCountUpgrade aplikace** balíčku.
3. Otevřete **C:\Temp\WordCountUpgrade\ApplicationManifest.xml** v textovém editoru.
4. V hello **ApplicationManifest** element, změna hello **ApplicationTypeVersion** atribut příliš**"1.1.0"**.  Tím se aktualizuje hello číslo verze aplikace hello.
5. Uložte soubor ApplicationManifest.xml hello změnit.
6. Spusťte následující skript prostředí PowerShell jako správce hello toocopy hello aplikace toohello image store:

```powershell
# Deploy hello WordCount and upgrade applications
$applicationPathWordCount = "C:\Temp\WordCount"
$applicationPathUpgrade = "C:\Temp\WordCountUpgrade"

# LOCAL:
$imageStoreConnection = "file:C:\SfDevCluster\Data\ImageStoreShare"
$cluster = 'localhost:19000'

Connect-ServiceFabricCluster $cluster

Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $applicationPathWordCount -ImageStoreConnectionString $imageStoreConnection
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $applicationPathUpgrade -ImageStoreConnectionString $imageStoreConnection
```

Po dokončení hello skript prostředí PowerShell, tato aplikace je připravená toorun.

## <a name="example"></a>Příklad
Hello následující příklad ukazuje životního cyklu aplikace Service Fabric hello.

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Fabric.Health;
using System.Fabric.Query;
using System.IO;
using System.Net;
using System.Text;
using System.Web.Script.Serialization;

namespace ServiceFabricRestCaller
{
    class Program
    {
        static void Main(string[] args)
        {
            Uri clusterUri = new Uri("http://localhost:19080");
            string buildPathApplication = "WordCount";
            string applicationVersionNumber = "1.0.0";
            string buildPathUpgrade = "WordCountUpgrade";
            string updateVersionNumber = "1.1.0";

            Console.WriteLine("\nProvision hello 1.0.0 WordCount application for hello first time.");
            ProvisionAnApplication(clusterUri, buildPathApplication);
            Console.WriteLine("\nPress Enter tooget hello list of application types: ");
            Console.ReadLine();


            Console.WriteLine("\nGet hello list of application types.");
            GetListOfApplicationTypes(clusterUri);
            Console.WriteLine("\nPress Enter toocreate hello fabric:/WordCount application: ");
            Console.ReadLine();


            Console.WriteLine("\nCreate hello fabric:/WordCount application.");
            CreateApplication(clusterUri);
            Console.WriteLine("\nPress Enter tooget hello list of applications: ");
            Console.ReadLine();


            Console.WriteLine("\nGet hello list of applications.");
            GetApplicationList(clusterUri);
            Console.WriteLine("\nPress Enter tooprovision hello 1.1.0 upgrade toohello WordCount application: ");
            Console.ReadLine();


            Console.WriteLine("\nProvision hello 1.1.0 upgrade toohello WordCount application.");
            ProvisionAnApplication(clusterUri, buildPathUpgrade);
            Console.WriteLine("\nPress Enter tooget hello list of application types: ");
            Console.ReadLine();


            Console.WriteLine("\nGet hello list of application types.");
            GetListOfApplicationTypes(clusterUri);
            Console.WriteLine("\nPress Enter tooupgrade hello fabric:/WordCount application: ");
            Console.ReadLine();


            Console.WriteLine("\nUpgrade hello fabric:/WordCount application.");
            UpgradeApplicationByApplicationType(clusterUri);
            Console.WriteLine("\nPress Enter tooget hello list of applications: ");
            Console.ReadLine();


            Console.WriteLine("\nGet hello list of applications.");
            GetApplicationList(clusterUri);
            Console.WriteLine("\nPress Enter toodelete hello fabric:/WordCount application: ");
            Console.ReadLine();


            Console.WriteLine("\nDelete hello fabric:/WordCount application.");
            DeleteApplication(clusterUri);
            Console.WriteLine("\nPress Enter tooget hello list of applications: ");
            Console.ReadLine();


            Console.WriteLine("\nGet hello list of applications.");
            GetApplicationList(clusterUri);
            Console.WriteLine("\nPress Enter toounprovision hello WordCount 1.1.0 application: ");
            Console.ReadLine();


            Console.WriteLine("\nUnprovision hello WordCount 1.1.0 application.");
            UnprovisionAnApplication(clusterUri, updateVersionNumber);
            Console.WriteLine("\nPress Enter tooget hello list of application types: ");
            Console.ReadLine();


            Console.WriteLine("\nGet hello list of application types.");
            GetListOfApplicationTypes(clusterUri);
            Console.WriteLine("\nPress Enter toounprovision hello WordCount 1.0.0 application: ");
            Console.ReadLine();


            Console.WriteLine("\nUnprovision hello WordCount 1.0.0 application.");
            UnprovisionAnApplication(clusterUri, applicationVersionNumber);
            Console.WriteLine("\nPress Enter tooget hello final list of application types: ");
            Console.ReadLine();


            Console.WriteLine("\nGet hello final list of application types.");
            GetListOfApplicationTypes(clusterUri);
            Console.WriteLine("\nPress Enter tooend this program: ");
            Console.ReadLine();
        }

        #region Classes

        /// <summary>
        /// Class similar tooApplicationType. Designed for use with JavaScriptSerializer.
        /// </summary>
        public class AppType
        {
            public string Name { get; set; }
            public string Version { get; set; }
            public List<ApplicationParameter> DefaultParameterList { get; set; }
        }

        /// <summary>
        /// Class designed for use with JavaScriptSerializer.
        /// </summary>
        public class ApplicationInfo
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string TypeName { get; set; }
            public string TypeVersion { get; set; }
            public ApplicationStatus Status { get; set; }
            public List<Parameter> Parameters { get; set; }
            public HealthState HealthState { get; set; }
        }

        /// <summary>
        /// Class similar tooParameter. Designed for use with JavaScriptSerializer.
        /// </summary>
        public class Parameter
        {
            public string Name { get; set; }
            public string Value { get; set; }
        }

        #endregion


        #region Get List of Application Types (REST API)

        /// <summary>
        /// Gets hello list of application types.
        /// </summary>
        /// <param name="clusterUri">hello URI tooaccess hello cluster.</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool GetListOfApplicationTypes(Uri clusterUri)
        {
            // String toocapture hello response stream.
            string responseString = string.Empty;

            // Create hello request and add URL parameters.
            Uri requestUri = new Uri(clusterUri, string.Format("/ApplicationTypes?api-version={0}",
            "1.0"));    // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.Method = "GET";

            // Execute hello request and obtain hello response.
            try
            {
                using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                {
                    using (StreamReader streamReader = new StreamReader(response.GetResponseStream(), true))
                    {
                        // Capture hello response string.
                        responseString = streamReader.ReadToEnd();
                    }
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display hello error message.
                Console.WriteLine("Error getting hello list of application types:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }

            // Deserialize hello response string.
            JavaScriptSerializer jss = new JavaScriptSerializer();
            List<AppType> applicationTypes = jss.Deserialize<List<AppType>>(responseString);

            // Display application type information for each application type.
            Console.WriteLine("Application types:");
            foreach (AppType applicationType in applicationTypes)
            {
                Console.WriteLine("  Application Type:");
                Console.WriteLine("    Name: " + applicationType.Name);
                Console.WriteLine("    Version: " + applicationType.Version);
                Console.WriteLine("    Default Parameter List:");

                foreach (var parameter in applicationType.DefaultParameterList)
                {
                    Console.WriteLine("      Name: " + parameter.Name);
                    Console.WriteLine("      Value: " + parameter.Value);
                }
            }

            return true;
        }

        #endregion


        #region Provision an Application (REST API)

        /// <summary>
        /// Provisions an application toohello image store.
        /// </summary>
        /// <param name="clusterUri">hello URI tooaccess hello cluster.</param>
        /// <param name="applicationTypeBuildPath">hello application type build path ("WordCount" or "WordCountUpgrade").</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool ProvisionAnApplication(Uri clusterUri, string applicationTypeBuildPath)
        {
            // Create hello request and add URL parameters.
            Uri requestUri = new Uri(clusterUri, string.Format("/ApplicationTypes/$/Provision?api-version={0}",
                "1.0"));    // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.Method = "POST";
            request.ContentType = "application/json; charset=utf-8";

            // Create hello byte array that will become hello request body.
            string requestBody = "{\"ApplicationTypeBuildPath\":\"" + applicationTypeBuildPath + "\"}";
            byte[] requestBodyBytes = Encoding.UTF8.GetBytes(requestBody);
            request.ContentLength = requestBodyBytes.Length;

            // Stores hello response status code.
            HttpStatusCode statusCode;

            // Create hello request body.
            try
            {
                using (Stream requestStream = request.GetRequestStream())
                {
                    requestStream.Write(requestBodyBytes, 0, requestBodyBytes.Length);
                    requestStream.Close();

                    // Execute hello request and obtain hello response.
                    using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                    {
                        statusCode = response.StatusCode;
                    }
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display hello error message.
                Console.WriteLine("Error provisioning hello application:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }

            Console.WriteLine("Provision an Application response status = " + statusCode.ToString());
            return true;
        }

        #endregion

        #region Unprovision an Application (REST API)

        /// <summary>
        /// Unprovisions an application.
        /// </summary>
        /// <param name="clusterUri">hello URI tooaccess hello cluster.</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool UnprovisionAnApplication(Uri clusterUri, string versionToUnprovision)
        {
            // Create hello request and add URL parameters.
            Uri requestUri = new Uri(clusterUri, string.Format("/ApplicationTypes/{0}/$/Unprovision?api-version={1}",
                "WordCount",     // Application Type Name
                "1.0"));            // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.Method = "POST";
            request.ContentType = "application/json; charset=utf-8";

            // Stores hello response status code.
            HttpStatusCode statusCode;

            // Create hello byte array that will become hello request body.
            string requestBody = "{\"ApplicationTypeVersion\":\"" + versionToUnprovision + "\"}";
            byte[] requestBodyBytes = Encoding.UTF8.GetBytes(requestBody);
            request.ContentLength = requestBodyBytes.Length;

            // Create hello request body.
            try
            {
                using (Stream requestStream = request.GetRequestStream())
                {
                    requestStream.Write(requestBodyBytes, 0, requestBodyBytes.Length);
                    requestStream.Close();

                    // Execute hello request and obtain hello response.
                    using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                    {
                        statusCode = response.StatusCode;
                    }
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display hello error message.
                Console.WriteLine("Error unprovisioning hello application:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }

            Console.WriteLine("Unprovision an Application response status = " + statusCode.ToString());
            return true;
        }

        #endregion

        #region Get Application List (REST API)

        /// <summary>
        /// Gets hello list of applications.
        /// </summary>
        /// <param name="clusterUri">hello URI tooaccess hello cluster.</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool GetApplicationList(Uri clusterUri)
        {
            // String toocapture hello response stream.
            string responseString = string.Empty;

            // Create hello request and add URL parameters.
            Uri requestUri = new Uri(clusterUri, string.Format("/Applications?api-version={0}",
                "1.0")); // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.Method = "GET";

            // Execute hello request and obtain hello response.
            try
            {
                using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                {
                    using (StreamReader streamReader = new StreamReader(response.GetResponseStream(), true))
                    {
                        // Capture hello response string.
                        responseString = streamReader.ReadToEnd();
                    }
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display hello error message.
                Console.WriteLine("Error getting hello application list:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }


            // Deserialize hello response string.
            JavaScriptSerializer jss = new JavaScriptSerializer();
            List<ApplicationInfo> applicationInfos = jss.Deserialize<List<ApplicationInfo>>(responseString);

            // Display some application information for each application.
            Console.WriteLine("Application List:");
            foreach (ApplicationInfo applicationInfo in applicationInfos)
            {
                Console.WriteLine("  Application:");
                Console.WriteLine("    Id: " + applicationInfo.Id);
                Console.WriteLine("    Name: " + applicationInfo.Name);
                Console.WriteLine("    TypeName: " + applicationInfo.TypeName);
                Console.WriteLine("    TypeVersion: " + applicationInfo.TypeVersion);
                Console.WriteLine("    Status: " + applicationInfo.Status);
                Console.WriteLine("    HealthState: " + applicationInfo.HealthState);

                Console.WriteLine("    Parameters:");

                foreach (Parameter parameter in applicationInfo.Parameters)
                {
                    Console.WriteLine("      Parameter:");
                    Console.WriteLine("        Name: " + parameter.Name);
                    Console.WriteLine("        Value: " + parameter.Value);
                }
            }

            return true;
        }

        #endregion

        #region Create Application (REST API)

        /// <summary>
        /// Creates an application.
        /// </summary>
        /// <param name="clusterUri">hello URI tooaccess hello cluster.</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool CreateApplication(Uri clusterUri)
        {
            // String toocapture hello response stream.
            string responseString = string.Empty;

            // Stores hello response status code.
            HttpStatusCode statusCode;

            // Create hello request and add URL parameters.
            Uri requestUri = new Uri(clusterUri, string.Format("/Applications/$/Create?api-version={0}",
                "1.0"));    // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.ContentType = "text/json";
            request.Method = "POST";

            // Create hello byte array that will become hello request body.
            string requestBody = "{\"Name\":\"fabric:/WordCount\"," +
                                    "\"TypeName\":\"WordCount\"," +
                                    "\"TypeVersion\":\"1.0.0\"," +
                                    "\"ParameterList\":[]}";
            byte[] requestBodyBytes = Encoding.UTF8.GetBytes(requestBody);
            request.ContentLength = requestBodyBytes.Length;

            // Create hello request body.
            try
            {
                using (Stream requestStream = request.GetRequestStream())
                {
                    requestStream.Write(requestBodyBytes, 0, requestBodyBytes.Length);
                    requestStream.Close();

                    // Execute hello request and obtain hello response.
                    using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                    {
                        statusCode = response.StatusCode;
                    }
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display hello error message.
                Console.WriteLine("Error creating application:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }

            Console.WriteLine("Create Application response status = " + statusCode.ToString());

            return true;
        }

        #endregion


        #region Delete Application (REST API)

        /// <summary>
        /// Deletes an application.
        /// </summary>
        /// <param name="clusterUri">hello URI tooaccess hello cluster.</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool DeleteApplication(Uri clusterUri)
        {
            // Create hello request and add URL parameters.
            Uri requestUri = new Uri(clusterUri,
                string.Format("/Applications/{0}/$/Delete?api-version={1}",
                "WordCount",    // Application Name
                "1.0"));        // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.Method = "POST";
            request.ContentLength = 0;

            // Stores hello response status code.
            HttpStatusCode statusCode;

            // Execute hello request and obtain hello response.
            try
            {
                using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                {
                    statusCode = response.StatusCode;
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display hello error message.
                Console.WriteLine("Error deleting application:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }

            Console.WriteLine("Delete Application response status = " + statusCode.ToString());

            return true;
        }

        #endregion

        #region Upgrade Application by Application Type (REST API)

        /// <summary>
        /// Upgrades an application by application type.
        /// </summary>
        /// <param name="clusterUri">hello URI tooaccess hello cluster.</param>
        /// <returns>Returns true if successful; otherwise false.</returns>
        public static bool UpgradeApplicationByApplicationType(Uri clusterUri)
        {
            // String toocapture hello response stream.
            string responseString = string.Empty;

            // Stores hello response status code.
            HttpStatusCode statusCode;

            // Create hello request and add URL parameters.
            Uri requestUri = new Uri(clusterUri, string.Format("/Applications/{0}/$/Upgrade?api-version={1}",
                "WordCount",     // Application Name
                "1.0"));                // api-version

            HttpWebRequest request = (HttpWebRequest)WebRequest.Create(requestUri);
            request.ContentType = "text/json";
            request.Method = "POST";


            // Create hello Health Policy.
            string requestBody = "{\"Name\":\"fabric:/WordCount\"," +
                                    "\"TargetApplicationTypeVersion\":\"1.1.0\"," +
                                    "\"Parameters\":[]," +
                                    "\"UpgradeKind\":1," +
                                    "\"RollingUpgradeMode\":1," +
                                    "\"UpgradeReplicaSetCheckTimeoutInSeconds\":5," +
                                    "\"ForceRestart\":true," +
                                    "\"MonitoringPolicy\":" +
                                    "{\"FailureAction\":1," +
                                    "\"HealthCheckWaitDurationInMilliseconds\":\"5000\"," +
                                    "\"HealthCheckStableDurationInMilliseconds\":\"10000\"," +
                                    "\"HealthCheckRetryTimeoutInMilliseconds\":\"20000\"," +
                                    "\"UpgradeTimeoutInMilliseconds\":\"60000\"," +
                                    "\"UpgradeDomainTimeoutInMilliseconds\":\"30000\"}}";

            // Create hello byte array that will become hello request body.
            byte[] requestBodyBytes = Encoding.UTF8.GetBytes(requestBody);
            request.ContentLength = requestBodyBytes.Length;

            // Create hello request body.
            try
            {
                using (Stream requestStream = request.GetRequestStream())
                {
                    requestStream.Write(requestBodyBytes, 0, requestBodyBytes.Length);
                    requestStream.Close();

                    // Execute hello request and obtain hello response.
                    using (HttpWebResponse response = (HttpWebResponse)request.GetResponse())
                    {
                        statusCode = response.StatusCode;
                    }
                }
            }
            catch (WebException e)
            {
                // If there is a web exception, display hello error message.
                Console.WriteLine("Error upgrading application:");
                Console.WriteLine(e.Message);
                if (e.InnerException != null)
                    Console.WriteLine(e.InnerException.Message);
                return false;
            }
            catch (Exception e)
            {
                // If there is another kind of exception, throw it.
                throw (e);
            }

            Console.WriteLine("Update Application response status = " + statusCode.ToString());

            return true;
        }

        #endregion
    }
}
```


<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
[Životní cyklus aplikace Service Fabric](service-fabric-application-lifecycle.md)

