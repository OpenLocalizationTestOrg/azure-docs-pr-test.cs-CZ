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
# <a name="how-tooconfigure-tls-mutual-authentication-for-web-app"></a>Jak tooConfigure vzájemné ověřování TLS pro webovou aplikaci
## <a name="overview"></a>Přehled
Povolením různé typy ověřování pro něj můžete omezit přístup tooyour webové aplikace Azure. Jedním ze způsobů toodo je proto tooauthenticate pomocí klientského certifikátu, když je požadavek hello protokolem TLS/SSL. Tento mechanismus se označuje jako TLS vzájemné ověřování nebo ověřování certifikátu klienta a tento článek podrobně popisuje jak toosetup vaší webové aplikace toouse ověřování pomocí certifikátu klienta.

> **Poznámka:** při přístupu k webu prostřednictvím protokolu HTTP a HTTPS není, neobdržíte žádné klientský certifikát. Proto pokud vaše aplikace vyžaduje klientské certifikáty by neměl povolíte požadavky aplikace tooyour přes protokol HTTP.
> 
> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="configure-web-app-for-client-certificate-authentication"></a>Konfigurovat webovou aplikaci pro ověření certifikátu klienta
toosetup vaší webové aplikace toorequire klientské certifikáty, které že budete potřebovat tooadd hello clientCertEnabled lokality nastavení pro webové aplikace a nastavte ji tootrue. Toto nastavení není aktuálně k dispozici prostřednictvím hello prostředí pro správu v hello portál a hello REST API potřebovat tooaccomplish toobe použít.

Můžete použít hello [ARMClient nástroj](https://github.com/projectkudu/ARMClient) toomake ho snadno toocraft hello volání rozhraní REST API. Po přihlásit hello nástroj budete potřebovat tooissue hello následující příkaz:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose

Nahraďte všechno ve {} informace pro vaši webovou aplikaci a vytvoření souboru s názvem enableclientcert.json s hello následující JSON obsahu:

    {
        "location": "My Web App Location",
        "properties": {
            "clientCertEnabled": true
        }
    }

Ujistěte se, že hodnota hello toochange toowherever "umístění", webová aplikace je umístěná například – Sever střední USA nebo západní USA atd.

Můžete taky https://resources.azure.com tooflip hello `clientCertEnabled` vlastnost příliš`true`.

> **Poznámka:** ARMClient při spuštění z prostředí Powershell, budete potřebovat tooescape hello @ symbol pro hello soubor JSON s back značek '.
> 
> 

## <a name="accessing-hello-client-certificate-from-your-web-app"></a>Přístup k hello klientský certifikát z webové aplikace
Pokud používáte technologii ASP.NET a nakonfigurovat ověřování certifikátu klienta toouse vaší aplikace, bude k dispozici prostřednictvím hello hello certifikát **HttpRequest.ClientCertificate** vlastnost. Pro ostatní zásobníky aplikace bude k dispozici ve vaší aplikaci pomocí kódováním base64 hodnoty v hlavičce žádosti "X-směrování žádostí na aplikace-ClientCert" hello hello klientského certifikátu. Aplikace můžete vytvořit certifikát od této hodnoty a použít ho pro účely ověřování a autorizace ve vaší aplikaci.

## <a name="special-considerations-for-certificate-validation"></a>Zvláštní upozornění pro ověření certifikátu
Hello klientský certifikát, který je odeslán toohello aplikace neprochází jakéhokoli ověřování podle platformy Azure Web Apps hello. Ověřit tento certifikát je hello odpovědnost hello webové aplikace. Zde je ukázka kódu ASP.NET, která ověřuje vlastnosti certifikátu pro účely ověření.

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
