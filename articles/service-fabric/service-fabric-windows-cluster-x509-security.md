---
title: "Zabezpečení clusteru služby Azure Service Fabric v systému Windows pomocí certifikátů | Microsoft Docs"
description: "Tento článek popisuje, jak zabezpečit komunikaci v rámci samostatné nebo privátní clusteru a také mezi klienty a cluster."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: fe0ed74c-9af5-44e9-8d62-faf1849af68c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dekapur
ms.openlocfilehash: 71ece1e43cc3c4ac3350cd59633065de06672420
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Zabezpečení clusteru s podporou samostatné do systému Windows pomocí certifikátů X.509
Tento článek popisuje, jak k zabezpečení komunikace mezi různými uzly clusteru Windows samostatné také jak k ověřování klientů, připojení k tomuto clusteru pomocí X.509 certifikáty. To zajišťuje, že můžete jenom autorizovaným uživatelům přístup ke clusteru, nasazené aplikace a provádět úlohy správy.  Certifikát zabezpečení by měly být povoleny v clusteru při vytvoření clusteru.  

Další informace o zabezpečení clusteru, jako jsou například – uzly zabezpečení, klientský uzel zabezpečení a řízení přístupu na základě rolí najdete v tématu [clusteru scénáře zabezpečení](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Certifikáty, které budete potřebovat?
Abyste mohli začít [Stáhnout balíček clusteru samostatné](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) s jedním z uzlů v clusteru. V staženého balíčku naleznete **ClusterConfig.X509.MultiMachine.json** souboru. Otevřete soubor a projděte si část pro **zabezpečení** pod **vlastnosti** části:

```JSON
"security": {
    "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
    "ClusterCredentialType": "X509",
    "ServerCredentialType": "X509",
    "CertificateInformation": {
        "ClusterCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },        
        "ClusterCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]"
            }
            ],
            "X509StoreName": "My"
        },
        "ServerCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ServerCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]"
            }
            ],
            "X509StoreName": "My"
        },
        "ClientCertificateThumbprints": [
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            },
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateCommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ReverseProxyCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ReverseProxyCertificateCommonNames": {
            "CommonNames": [
                {
                "CertificateCommonName": "[CertificateCommonName]"
                }
            ],
            "X509StoreName": "My"
        }
    }
},
```

Tato část popisuje certifikáty, které je třeba pro zabezpečení samostatný cluster Windows. Pokud zadáte certifikát clusteru, nastavte hodnotu **ClusterCredentialType** k  _**X509**_. Pokud zadáte certifikát serveru pro připojení mimo, nastavte **ServerCredentialType** k  _**X509**_. I když není povinné, doporučujeme mít oba tyto certifikáty pro cluster s podporou správně zabezpečené. Pokud nastavíte tyto hodnoty na *X509* pak je nutné také zadat odpovídající certifikáty nebo Service Fabric vyvolá výjimku. V některých scénářích pouze můžete zadat _ClientCertificateThumbprints_ nebo _ReverseProxyCertificate_. V těchto scénářích, není nutné nastavovat _ClusterCredentialType_ nebo _ServerCredentialType_ k _X509_.


> [!NOTE]
> A [kryptografický otisk](https://en.wikipedia.org/wiki/Public_key_fingerprint) je primární Identita certifikátu. Čtení [postup načtení kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695.aspx) a zjistěte, kryptografický otisk certifikáty, které vytvoříte.
> 
> 

Následující tabulka uvádí certifikáty, které budete potřebovat na vaše nastavení clusteru:

| **Nastavení CertificateInformation** | **Popis** |
| --- | --- |
| ClusterCertificate |Vhodné pro testovací prostředí. Tento certifikát je vyžadován k zabezpečení komunikace mezi uzly v clusteru. Můžete použít dvě různé certifikáty, primární a sekundární pro upgrade. Kryptografický otisk certifikátu primární v nastavení **kryptografický otisk** části a u sekundární v **ThumbprintSecondary** proměnné. |
| ClusterCertificateCommonNames |Nedoporučuje pro produkční prostředí. Tento certifikát je vyžadován k zabezpečení komunikace mezi uzly v clusteru. Můžete použít jednu nebo dvě clusteru běžné názvy certifikátů. |
| ServerCertificate |Vhodné pro testovací prostředí. Tento certifikát je pro klienta zobrazí při pokusu o připojení k tomuto clusteru. Pro usnadnění práce, můžete použít stejný certifikát pro *ClusterCertificate* a *ServerCertificate*. Dva certifikáty jiný server, primární a sekundární můžete použít pro upgrade. Kryptografický otisk certifikátu primární v nastavení **kryptografický otisk** části a u sekundární v **ThumbprintSecondary** proměnné. |
| ServerCertificateCommonNames |Nedoporučuje pro produkční prostředí. Tento certifikát je pro klienta zobrazí při pokusu o připojení k tomuto clusteru. Pro usnadnění práce, můžete použít stejný certifikát pro *ClusterCertificateCommonNames* a *ServerCertificateCommonNames*. Můžete použít jednu nebo dvě certifikát běžné názvy serverů. |
| ClientCertificateThumbprints |To je sada certifikáty, které chcete instalovat na ověřené klienty. Může mít řadu různých klientské certifikáty, které jsou nainstalované v počítačích, které chcete povolit přístup ke clusteru. Kryptografický otisk každý certifikát v nastavení **CertificateThumbprint** proměnné. Pokud nastavíte **IsAdmin** k *true*, pak klienta se tento certifikát nainstalován, můžete provést správce správy aktivit v clusteru. Pokud **IsAdmin** je *false*, klient s tímto certifikátem můžete provádět jenom akce povolené pro uživatelská přístupová práva, obvykle jen pro čtení. Další informace o rolích najdete v tématu [řízení přístupu (RBAC) na základě Role](service-fabric-cluster-security.md#role-based-access-control-rbac) |
| ClientCertificateCommonNames |Běžný název první klientský certifikát pro nastavit **CertificateCommonName**. **CertificateIssuerThumbprint** je kryptografický otisk pro vystavitele tohoto certifikátu. Čtení [práce s certifikáty](https://msdn.microsoft.com/library/ms731899.aspx) Další informace o běžné názvy a vystavitele. |
| ReverseProxyCertificate |Vhodné pro testovací prostředí. Toto je volitelné certifikát, který může být zadán, pokud chcete zabezpečit vaše [Reverse Proxy](service-fabric-reverseproxy.md). Ujistěte se, že reverseProxyEndpointPort je nastaven v nodeTypes, pokud používáte tento certifikát. |
| ReverseProxyCertificateCommonNames |Nedoporučuje pro produkční prostředí. Toto je volitelné certifikát, který může být zadán, pokud chcete zabezpečit vaše [Reverse Proxy](service-fabric-reverseproxy.md). Ujistěte se, že reverseProxyEndpointPort je nastaven v nodeTypes, pokud používáte tento certifikát. |

Tady je příklad konfigurace clusteru kde byly zadány clusteru, serveru a klientské certifikáty. Pamatujte, že pro cluster nebo server / reverseProxy certifikáty, kryptografický otisk a běžný název nejsou povoleny společně nakonfigurovat pro stejný typ certifikátu.

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myClusterCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ServerCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myServerCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpointPort": "19001",
            "leaseDriverEndpointPort": "19002",
            "serviceConnectionEndpointPort": "19003",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="certificate-roll-over"></a>Převrácení certifikátu
Při použití běžný název certifikátu místo kryptografický otisk, certifikát kumulativní přes nevyžaduje upgrade konfigurace clusteru.
Pokud certifikát kumulativní přes zahrnuje vystavitele mění, uložte zachovat původní Vystavitel certifikátu v certifikát minimálně 2 hodiny po instalaci nového vystavitele certifikátu.

## <a name="acquire-the-x509-certificates"></a>Získat certifikáty X.509
Pro zabezpečení komunikace v rámci clusteru, bude nejprve nutné k získání certifikátů X.509 pro uzly clusteru. Kromě toho pokud chcete omezit připojení k tomuto clusteru na autorizované počítače nebo uživatele, musíte získat a nainstalovat certifikáty pro klientské počítače.

Pro clustery, které jsou spuštěné úlohy v produkčním prostředí, měli byste použít [certifikační autoritou (CA)](https://en.wikipedia.org/wiki/Certificate_authority) podepsaný certifikát X.509 k zabezpečení clusteru. Podrobnosti o získání těchto certifikátů, přejděte na [postupy: získání certifikátu](http://msdn.microsoft.com/library/aa702761.aspx).

Pro clustery, které používáte pro testovací účely můžete použít certifikát podepsaný svým držitelem.

## <a name="optional-create-a-self-signed-certificate"></a>Volitelné: Vytvořit certifikát podepsaný svým držitelem
Vytvoření certifikátu podepsaného svým držitelem, který může být správně zabezpečená jedním ze způsobů je používat *CertSetup.ps1* skript ve složce Service Fabric SDK v adresáři *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*. Upravit tento soubor a změnit výchozí název certifikátu (vyhledejte hodnotu *CN = ServiceFabricDevClusterCert*). Spusťte tento skript jako `.\CertSetup.ps1 -Install`.

Nyní exportujte certifikát do souboru PFX chráněný heslem. Nejdřív získáte kryptografický otisk certifikátu. Z *spustit* nabídky, spusťte *spravovat certifikáty počítače*. Přejděte na **Místní počítač\Osobní** složky a najít certifikát, který jste právě vytvořili. Dvakrát klikněte na certifikát, který chcete otevřít, vyberte *podrobnosti* kartě a přejděte dolů k položce *kryptografický otisk* pole. Zkopírujte hodnotu kryptografického otisku do příkaz prostředí PowerShell následující po odebrání prostory.  Změna `String` hodnotu na vhodný zabezpečené heslo, aby ho ochránil a spusťte následující příkazy v prostředí PowerShell:

```powershell   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

Chcete-li zobrazit podrobnosti o certifikát nainstalovaný na počítači spuštěním následujícího příkazu Powershellu:

```powershell
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Případně, pokud máte předplatné Azure, postupujte podle kroků v části [přidejte certifikáty do Key Vault](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-the-certificates"></a>Nainstalujte certifikáty
Jakmile máte autority, můžete je nainstalovat na uzlech clusteru. Uzly musí mít nejnovější prostředí Windows PowerShell 3.x na nich být nainstalovaný. Musíte opakujte tyto kroky na každém uzlu clusteru a Server certifikátů a všechny sekundární certifikátů.

1. Zkopírujte soubory .pfx do uzlu.
2. Otevřete okno prostředí PowerShell jako správce a zadejte následující příkazy. Nahraďte *$pswd* s heslem, které jste použili k vytvoření tohoto certifikátu. Nahraďte *$PfxFilePath* s úplnou cestou .pfx zkopíruje na tento uzel.
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. Teď nastavte řízení přístupu na tento certifikát tak, aby Service Fabric procesu, který běží pod účtem Network Service, můžete použít tak, že spustíte následující skript. Zadejte kryptografický otisk certifikátu a "Síťová služba" pro účet služby. Můžete zkontrolovat, zda jsou seznamy ACL v certifikátu správné otevřením certifikátu v *spustit* > *spravovat certifikáty počítače* a prohlížení *všechny úlohy* > *spravovat privátní klíče*.
   
    ```powershell
    param
    (
    [Parameter(Position=1, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$pfxThumbPrint,
   
    [Parameter(Position=2, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$serviceAccount
    )
   
    $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; }
   
    # Specify the user, the permissions and the permission type
    $permission = "$($serviceAccount)","FullControl","Allow"
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of the machine related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get the current acl of the private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add the new ace to the acl of the private key
    $acl.SetAccessRule($accessRule)
   
    # Write back the new acl
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe the access rights currently assigned to this certificate.
    get-acl $keyFullPath| fl
    ```
4. Opakujte předchozí kroky pro každý certifikát serveru. Tyto kroky můžete použít také k instalaci klientské certifikáty na počítačích, které chcete povolit přístup ke clusteru.

## <a name="create-the-secure-cluster"></a>Vytvoření zabezpečeného clusteru
Po dokončení konfigurace **zabezpečení** části **ClusterConfig.X509.MultiMachine.json** souboru, můžete přejít k [vytvořit cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) oddílu konfigurace uzlů a vytvořit samostatný cluster. Nezapomeňte použít **ClusterConfig.X509.MultiMachine.json** souboru při vytvoření clusteru. Například váš příkaz může vypadat následovně:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

Jakmile máte zabezpečené samostatné Windows clusteru úspěšně spuštěna, a nastavili ověřené klientům připojit se k němu, postupujte podle části [připojení ke clusteru zabezpečené pomocí prostředí PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) připojení k tomuto. Například:

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

Potom můžete spustit další příkazy prostředí PowerShell pro práci s tímto clusterem. Například [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) zobrazíte seznam uzlů na tomto zabezpečení clusteru.


Odebrat cluster, připojit k uzlu v clusteru, kam jste stáhli balíček Service Fabric, otevřete příkazový řádek a přejděte do složky balíčku. Nyní spusťte následující příkaz:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> Nesprávný certifikát konfigurace mohou zabránit objevuje během nasazení clusteru. Samoobslužné diagnostikovat problémy se zabezpečením, podívejte se prosím ve skupině Prohlížeč událostí *protokoly aplikací a služeb* > *Microsoft Service Fabric*.
> 
> 

