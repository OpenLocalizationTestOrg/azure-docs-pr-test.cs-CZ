---
title: "aaaHow tooConfigure vzájemné ověřování TLS pro webovou aplikaci"
description: "Zjistěte, jak tooconfigure vaší webové aplikace toouse klientský certifikát ověřování na TLS."
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
ms.openlocfilehash: 8aeb9b35058fac50b8b38f6428207ad4a82d8637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-tls-mutual-authentication-for-web-app"></a><span data-ttu-id="9c7f4-103">Jak tooConfigure vzájemné ověřování TLS pro webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="9c7f4-103">How tooConfigure TLS Mutual Authentication for Web App</span></span>
## <a name="overview"></a><span data-ttu-id="9c7f4-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="9c7f4-104">Overview</span></span>
<span data-ttu-id="9c7f4-105">Povolením různé typy ověřování pro něj můžete omezit přístup tooyour webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-105">You can restrict access tooyour Azure web app by enabling different types of authentication for it.</span></span> <span data-ttu-id="9c7f4-106">Jedním ze způsobů toodo je proto tooauthenticate pomocí klientského certifikátu, když je požadavek hello protokolem TLS/SSL.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-106">One way toodo so is tooauthenticate using a client certificate when hello request is over TLS/SSL.</span></span> <span data-ttu-id="9c7f4-107">Tento mechanismus se označuje jako TLS vzájemné ověřování nebo ověřování certifikátu klienta a tento článek podrobně popisuje jak toosetup vaší webové aplikace toouse ověřování pomocí certifikátu klienta.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-107">This mechanism is called TLS mutual authentication or client certificate authentication and this article will detail how toosetup your web app toouse client certificate authentication.</span></span>

> <span data-ttu-id="9c7f4-108">**Poznámka:** při přístupu k webu prostřednictvím protokolu HTTP a HTTPS není, neobdržíte žádné klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-108">**Note:** If you access your site over HTTP and not HTTPS, you will not receive any client certificate.</span></span> <span data-ttu-id="9c7f4-109">Proto pokud vaše aplikace vyžaduje klientské certifikáty by neměl povolíte požadavky aplikace tooyour přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-109">So if your application requires client certificates you should not allow requests tooyour application over HTTP.</span></span>
> 
> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="configure-web-app-for-client-certificate-authentication"></a><span data-ttu-id="9c7f4-110">Konfigurovat webovou aplikaci pro ověření certifikátu klienta</span><span class="sxs-lookup"><span data-stu-id="9c7f4-110">Configure Web App for Client Certificate Authentication</span></span>
<span data-ttu-id="9c7f4-111">toosetup vaší webové aplikace toorequire klientské certifikáty, které že budete potřebovat tooadd hello clientCertEnabled lokality nastavení pro webové aplikace a nastavte ji tootrue.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-111">toosetup your web app toorequire client certificates you need tooadd hello clientCertEnabled site setting for your web app and set it tootrue.</span></span> <span data-ttu-id="9c7f4-112">Toto nastavení není aktuálně k dispozici prostřednictvím hello prostředí pro správu v hello portál a hello REST API potřebovat tooaccomplish toobe použít.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-112">This setting is not currently available through hello management experience in hello Portal, and hello REST API will need toobe used tooaccomplish this.</span></span>

<span data-ttu-id="9c7f4-113">Můžete použít hello [ARMClient nástroj](https://github.com/projectkudu/ARMClient) toomake ho snadno toocraft hello volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-113">You can use hello [ARMClient tool](https://github.com/projectkudu/ARMClient) toomake it easy toocraft hello REST API call.</span></span> <span data-ttu-id="9c7f4-114">Po přihlásit hello nástroj budete potřebovat tooissue hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9c7f4-114">After you log in with hello tool you will need tooissue hello following command:</span></span>

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose

<span data-ttu-id="9c7f4-115">Nahraďte všechno ve {} informace pro vaši webovou aplikaci a vytvoření souboru s názvem enableclientcert.json s hello následující JSON obsahu:</span><span class="sxs-lookup"><span data-stu-id="9c7f4-115">replacing everything in {} with information for your web app and creating a file called enableclientcert.json with hello following JSON content:</span></span>

    {
        "location": "My Web App Location",
        "properties": {
            "clientCertEnabled": true
        }
    }

<span data-ttu-id="9c7f4-116">Ujistěte se, že hodnota hello toochange toowherever "umístění", webová aplikace je umístěná například – Sever střední USA nebo západní USA atd.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-116">Make sure toochange hello value of "location" toowherever your web app is located e.g. North Central US or West US etc.</span></span>

<span data-ttu-id="9c7f4-117">Můžete taky https://resources.azure.com tooflip hello `clientCertEnabled` vlastnost příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-117">You can also use https://resources.azure.com tooflip hello `clientCertEnabled` property too`true`.</span></span>

> <span data-ttu-id="9c7f4-118">**Poznámka:** ARMClient při spuštění z prostředí Powershell, budete potřebovat tooescape hello @ symbol pro hello soubor JSON s back značek '.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-118">**Note:** If you run ARMClient from Powershell, you will need tooescape hello @ symbol for hello JSON file with a back tick \`.</span></span>
> 
> 

## <a name="accessing-hello-client-certificate-from-your-web-app"></a><span data-ttu-id="9c7f4-119">Přístup k hello klientský certifikát z webové aplikace</span><span class="sxs-lookup"><span data-stu-id="9c7f4-119">Accessing hello Client Certificate From Your Web App</span></span>
<span data-ttu-id="9c7f4-120">Pokud používáte technologii ASP.NET a nakonfigurovat ověřování certifikátu klienta toouse vaší aplikace, bude k dispozici prostřednictvím hello hello certifikát **HttpRequest.ClientCertificate** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-120">If you are using ASP.NET and configure your app toouse client certificate authentication, hello certificate will be available through hello **HttpRequest.ClientCertificate** property.</span></span> <span data-ttu-id="9c7f4-121">Pro ostatní zásobníky aplikace bude k dispozici ve vaší aplikaci pomocí kódováním base64 hodnoty v hlavičce žádosti "X-směrování žádostí na aplikace-ClientCert" hello hello klientského certifikátu.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-121">For other application stacks, hello client cert will be available in your app through a base64 encoded value in hello "X-ARR-ClientCert" request header.</span></span> <span data-ttu-id="9c7f4-122">Aplikace můžete vytvořit certifikát od této hodnoty a použít ho pro účely ověřování a autorizace ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-122">Your application can create a certificate from this value and then use it for authentication and authorization purposes in your application.</span></span>

## <a name="special-considerations-for-certificate-validation"></a><span data-ttu-id="9c7f4-123">Zvláštní upozornění pro ověření certifikátu</span><span class="sxs-lookup"><span data-stu-id="9c7f4-123">Special Considerations for Certificate Validation</span></span>
<span data-ttu-id="9c7f4-124">Hello klientský certifikát, který je odeslán toohello aplikace neprochází jakéhokoli ověřování podle platformy Azure Web Apps hello.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-124">hello client certificate that is sent toohello application does not go through any validation by hello Azure Web Apps platform.</span></span> <span data-ttu-id="9c7f4-125">Ověřit tento certifikát je hello odpovědnost hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-125">Validating this certificate is hello responsibility of hello web app.</span></span> <span data-ttu-id="9c7f4-126">Zde je ukázka kódu ASP.NET, která ověřuje vlastnosti certifikátu pro účely ověření.</span><span class="sxs-lookup"><span data-stu-id="9c7f4-126">Here is sample ASP.NET code that validates certificate properties for authentication purposes.</span></span>

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
            // Read hello certificate from hello header into an X509Certificate2 object
            // Display properties of hello certificate on hello page
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
                // In this example we will only accept hello certificate as a valid certificate if all hello conditions below are met:
                // 1. hello certificate is not expired and is active for hello current time on server.
                // 2. hello subject name of hello certificate has hello common name nildevecc
                // 3. hello issuer name of hello certificate has hello common name nildevecc and organization name Microsoft Corp
                // 4. hello thumbprint of hello certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained tooa Trusted Root Authority (or revoked) on hello server 
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

                // If you also want tootest if hello certificate chains tooa Trusted Root Authority you can uncomment hello code below
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
