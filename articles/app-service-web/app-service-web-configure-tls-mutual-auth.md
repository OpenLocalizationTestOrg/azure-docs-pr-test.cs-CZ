---
title: "Jak nakonfigurovat vzájemné ověřování protokolu TLS pro webovou aplikaci"
description: "Informace o konfiguraci webové aplikace na protokol TLS použít ověřování certifikátu klienta."
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: jimbe
ms.assetid: cd1d15d3-2d9e-4502-9f11-a306dac4453a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2016
ms.author: naziml
ms.openlocfilehash: db69852cffd1ff331ac4a640b04ea4360d00bf75
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-configure-tls-mutual-authentication-for-web-app"></a><span data-ttu-id="14a3a-103">Jak nakonfigurovat vzájemné ověřování protokolu TLS pro webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="14a3a-103">How To Configure TLS Mutual Authentication for Web App</span></span>
## <a name="overview"></a><span data-ttu-id="14a3a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="14a3a-104">Overview</span></span>
<span data-ttu-id="14a3a-105">Povolením různé typy ověřování pro něj můžete omezit přístup k vaší webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="14a3a-105">You can restrict access to your Azure web app by enabling different types of authentication for it.</span></span> <span data-ttu-id="14a3a-106">Jedním ze způsobů k tomu je k ověření pomocí klientského certifikátu, pokud je požadavek protokolem TLS/SSL.</span><span class="sxs-lookup"><span data-stu-id="14a3a-106">One way to do so is to authenticate using a client certificate when the request is over TLS/SSL.</span></span> <span data-ttu-id="14a3a-107">Tento mechanismus se nazývají vzájemné ověřování TLS nebo klientský certifikát, ověřování a tento článek podrobně popisuje postup nastavení webové aplikace použít ověřování certifikátu klienta.</span><span class="sxs-lookup"><span data-stu-id="14a3a-107">This mechanism is called TLS mutual authentication or client certificate authentication and this article will detail how to setup your web app to use client certificate authentication.</span></span>

> <span data-ttu-id="14a3a-108">**Poznámka:** při přístupu k webu prostřednictvím protokolu HTTP a HTTPS není, neobdržíte žádné klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="14a3a-108">**Note:** If you access your site over HTTP and not HTTPS, you will not receive any client certificate.</span></span> <span data-ttu-id="14a3a-109">Proto pokud vaše aplikace vyžaduje klientské certifikáty neměli povolit požadavky do vaší aplikace prostřednictvím protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="14a3a-109">So if your application requires client certificates you should not allow requests to your application over HTTP.</span></span>
> 
> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="configure-web-app-for-client-certificate-authentication"></a><span data-ttu-id="14a3a-110">Konfigurovat webovou aplikaci pro ověření certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="14a3a-110">Configure Web App for Client Certificate Authentication</span></span>
<span data-ttu-id="14a3a-111">K nastavení vaší webové aplikace na klientské certifikáty vyžadovat musíte přidat nastavení lokality clientCertEnabled pro vaši webovou aplikaci a nastavte ji na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="14a3a-111">To setup your web app to require client certificates you need to add the clientCertEnabled site setting for your web app and set it to true.</span></span> <span data-ttu-id="14a3a-112">Toto nastavení není aktuálně k dispozici prostřednictvím prostředí pro správu na portálu a bude nutné se k tomu použít rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="14a3a-112">This setting is not currently available through the management experience in the Portal, and the REST API will need to be used to accomplish this.</span></span>

<span data-ttu-id="14a3a-113">Můžete použít [ARMClient nástroj](https://github.com/projectkudu/ARMClient) snadno vytvořit volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="14a3a-113">You can use the [ARMClient tool](https://github.com/projectkudu/ARMClient) to make it easy to craft the REST API call.</span></span> <span data-ttu-id="14a3a-114">Jakmile se přihlásíte pomocí nástroje budete muset vydejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="14a3a-114">After you log in with the tool you will need to issue the following command:</span></span>

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose

<span data-ttu-id="14a3a-115">Nahraďte všechno ve {} informace pro vaši webovou aplikaci a vytvoření souboru s názvem enableclientcert.json následujícím textem JSON obsahu:</span><span class="sxs-lookup"><span data-stu-id="14a3a-115">replacing everything in {} with information for your web app and creating a file called enableclientcert.json with the following JSON content:</span></span>

    {
        "location": "My Web App Location",
        "properties": {
            "clientCertEnabled": true
        }
    }

<span data-ttu-id="14a3a-116">Ujistěte se, chcete-li změnit hodnotu "umístění" k kdekoli vaší webové aplikace se nachází například – Sever střední USA nebo západní USA atd.</span><span class="sxs-lookup"><span data-stu-id="14a3a-116">Make sure to change the value of "location" to wherever your web app is located e.g. North Central US or West US etc.</span></span>

<span data-ttu-id="14a3a-117">Můžete také použít https://resources.azure.com k převrácení `clientCertEnabled` vlastnost `true`.</span><span class="sxs-lookup"><span data-stu-id="14a3a-117">You can also use https://resources.azure.com to flip the `clientCertEnabled` property to `true`.</span></span>

> <span data-ttu-id="14a3a-118">**Poznámka:** ARMClient při spuštění z prostředí Powershell, budete muset vyhnuli symbol @ pro soubor JSON s back značek '.</span><span class="sxs-lookup"><span data-stu-id="14a3a-118">**Note:** If you run ARMClient from Powershell, you will need to escape the @ symbol for the JSON file with a back tick \`.</span></span>
> 
> 

## <a name="accessing-the-client-certificate-from-your-web-app"></a><span data-ttu-id="14a3a-119">Přístup k certifikátu klienta z vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="14a3a-119">Accessing the Client Certificate From Your Web App</span></span>
<span data-ttu-id="14a3a-120">Pokud používáte technologii ASP.NET a nakonfigurovat v aplikaci použít ověřování certifikátu klienta, certifikát je k dispozici prostřednictvím **HttpRequest.ClientCertificate** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="14a3a-120">If you are using ASP.NET and configure your app to use client certificate authentication, the certificate will be available through the **HttpRequest.ClientCertificate** property.</span></span> <span data-ttu-id="14a3a-121">Pro ostatní zásobníky aplikace bude k dispozici ve vaší aplikaci pomocí kódováním base64 hodnoty v hlavičce žádosti "X-směrování žádostí na aplikace-ClientCert" klientského certifikátu.</span><span class="sxs-lookup"><span data-stu-id="14a3a-121">For other application stacks, the client cert will be available in your app through a base64 encoded value in the "X-ARR-ClientCert" request header.</span></span> <span data-ttu-id="14a3a-122">Aplikace můžete vytvořit certifikát od této hodnoty a použít ho pro účely ověřování a autorizace ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="14a3a-122">Your application can create a certificate from this value and then use it for authentication and authorization purposes in your application.</span></span>

## <a name="special-considerations-for-certificate-validation"></a><span data-ttu-id="14a3a-123">Zvláštní upozornění pro ověření certifikátu</span><span class="sxs-lookup"><span data-stu-id="14a3a-123">Special Considerations for Certificate Validation</span></span>
<span data-ttu-id="14a3a-124">Klientský certifikát, který je odeslán do aplikace neprochází žádné ověření platformou Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="14a3a-124">The client certificate that is sent to the application does not go through any validation by the Azure Web Apps platform.</span></span> <span data-ttu-id="14a3a-125">Ověřit tento certifikát má na starosti webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="14a3a-125">Validating this certificate is the responsibility of the web app.</span></span> <span data-ttu-id="14a3a-126">Zde je ukázka kódu ASP.NET, která ověřuje vlastnosti certifikátu pro účely ověření.</span><span class="sxs-lookup"><span data-stu-id="14a3a-126">Here is sample ASP.NET code that validates certificate properties for authentication purposes.</span></span>

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;

                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;

                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }