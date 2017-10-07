---
title: "aaaSecure Azure Service Fabric clusteru v systému Windows pomocí certifikátů | Microsoft Docs"
description: "Tento článek popisuje, jak toosecure komunikaci v rámci hello samostatné, nebo privátní clusteru také mezi klienty a hello clusteru."
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
ms.openlocfilehash: f0d411963615349a84edfc8125dec4ee5908f146
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Zabezpečení clusteru s podporou samostatné do systému Windows pomocí certifikátů X.509
Tento článek popisuje, jak toosecure hello komunikace mezi hello různé uzly clusteru Windows samostatné taky, jak tooauthenticate klienti připojení clusteru toothis pomocí certifikátů X.509. To zajišťuje, jenom Autorizovaní uživatelé mají přístup ke clusteru hello hello nasazené aplikace a provádět úlohy správy.  Certifikát zabezpečení může být povoleno v clusteru hello při vytvoření clusteru hello.  

Další informace o zabezpečení clusteru, jako jsou například – uzly zabezpečení, klientský uzel zabezpečení a řízení přístupu na základě rolí najdete v tématu [clusteru scénáře zabezpečení](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Certifikáty, které budete potřebovat?
toostart, [Stáhnout balíček clusteru samostatné hello](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) tooone hello uzlů v clusteru. V hello stáhli balíček, najdete **ClusterConfig.X509.MultiMachine.json** souboru. Otevřete soubor hello a projděte si část hello pro **zabezpečení** pod hello **vlastnosti** části:

```JSON
"security": {
    "metadata": "hello Credential type X509 indicates this is cluster is secured using X509 Certificates. hello thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
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

Tato část popisuje hello certifikáty, které je třeba pro zabezpečení samostatný cluster Windows. Pokud zadáte certifikát clusteru, nastavte hodnotu hello **ClusterCredentialType** too_**X509**_. Pokud zadáte certifikát serveru pro připojení mimo, nastavte hello **ServerCredentialType** příliš_**X509**_. I když není povinné, doporučujeme toohave oba tyto certifikáty pro cluster s podporou správně zabezpečené. Pokud nastavíte tyto hodnoty příliš*X509* pak musíte zadat také hello odpovídající certifikáty nebo Service Fabric vyvolá výjimku. V některých případech je vhodné pouze toospecify hello _ClientCertificateThumbprints_ nebo _ReverseProxyCertificate_. V těchto scénářích, není nutné nastavovat _ClusterCredentialType_ nebo _ServerCredentialType_ too_X509_.


> [!NOTE]
> A [kryptografický otisk](https://en.wikipedia.org/wiki/Public_key_fingerprint) je primární identita hello certifikátu. Čtení [jak tooretrieve kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695.aspx) toofind out hello kryptografický otisk hello certifikáty, které vytvoříte.
> 
> 

Hello následující tabulka uvádí hello certifikáty, které budete potřebovat na vaše nastavení clusteru:

| **Nastavení CertificateInformation** | **Popis** |
| --- | --- |
| ClusterCertificate |Vhodné pro testovací prostředí. Tento certifikát se vyžaduje toosecure hello komunikaci mezi hello uzly v clusteru. Můžete použít dvě různé certifikáty, primární a sekundární pro upgrade. Nastavit hello kryptografický otisk certifikátu primární hello v hello **kryptografický otisk** části a který hello sekundární v hello **ThumbprintSecondary** proměnné. |
| ClusterCertificateCommonNames |Nedoporučuje pro produkční prostředí. Tento certifikát se vyžaduje toosecure hello komunikaci mezi hello uzly v clusteru. Můžete použít jednu nebo dvě clusteru běžné názvy certifikátů. |
| ServerCertificate |Vhodné pro testovací prostředí. Tento certifikát se zobrazí toohello klienta, když se ho pokusí tooconnect toothis clusteru. Pro usnadnění práce, můžete toouse hello stejný certifikát pro *ClusterCertificate* a *ServerCertificate*. Dva certifikáty jiný server, primární a sekundární můžete použít pro upgrade. Nastavit hello kryptografický otisk certifikátu primární hello v hello **kryptografický otisk** části a který hello sekundární v hello **ThumbprintSecondary** proměnné. |
| ServerCertificateCommonNames |Nedoporučuje pro produkční prostředí. Tento certifikát se zobrazí toohello klienta, když se ho pokusí tooconnect toothis clusteru. Pro usnadnění práce, můžete toouse hello stejný certifikát pro *ClusterCertificateCommonNames* a *ServerCertificateCommonNames*. Můžete použít jednu nebo dvě certifikát běžné názvy serverů. |
| ClientCertificateThumbprints |To je sada certifikáty, které chcete tooinstall na klientech hello ověření. Může mít řadu různých klientských certifikátů nainstalovaných na hello počítače, které chcete clusteru toohello tooallow přístupu. Nastaví kryptografický otisk hello každý certifikát v hello **CertificateThumbprint** proměnné. Pokud nastavíte hello **IsAdmin** příliš*true*, a poté klienta hello se tento certifikát nainstalován můžete proveďte správce správy aktivit v clusteru hello. Pokud hello **IsAdmin** je *false*, hello klienta s tímto certifikátem můžete provádět jenom akce hello povolené pro uživatelská přístupová práva, obvykle jen pro čtení. Další informace o rolích najdete v tématu [řízení přístupu (RBAC) na základě Role](service-fabric-cluster-security.md#role-based-access-control-rbac) |
| ClientCertificateCommonNames |Sada hello běžný název hello první klientský certifikát pro hello **CertificateCommonName**. Hello **CertificateIssuerThumbprint** je hello kryptografický otisk pro hello vystavitele tohoto certifikátu. Čtení [práce s certifikáty](https://msdn.microsoft.com/library/ms731899.aspx) tooknow více informací o běžné názvy a hello vystavitele. |
| ReverseProxyCertificate |Vhodné pro testovací prostředí. Toto je volitelné certifikát, který může být zadán, pokud chcete, aby toosecure vaše [Reverse Proxy](service-fabric-reverseproxy.md). Ujistěte se, že reverseProxyEndpointPort je nastaven v nodeTypes, pokud používáte tento certifikát. |
| ReverseProxyCertificateCommonNames |Nedoporučuje pro produkční prostředí. Toto je volitelné certifikát, který může být zadán, pokud chcete, aby toosecure vaše [Reverse Proxy](service-fabric-reverseproxy.md). Ujistěte se, že reverseProxyEndpointPort je nastaven v nodeTypes, pokud používáte tento certifikát. |

Tady je příklad konfigurace clusteru kde byly zadány hello clusteru, serveru a klientské certifikáty. Pamatujte, že pro cluster nebo server / reverseProxy certifikáty, kryptografický otisk a běžný název nejsou povoleny toobe nakonfigurované společně pro hello stejný typ certifikátu.

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace hello localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace hello localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace hello localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "hello Credential type X509 indicates this is cluster is secured using X509 Certificates. hello thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
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
Pokud certifikát kumulativní přes zahrnuje vystavitele mění, prosím zachovat hello staré vystavitele certifikátu v úložišti certifikátů hello minimálně 2 hodiny po instalaci nového vystavitele certifikátu hello.

## <a name="acquire-hello-x509-certificates"></a>Získat certifikáty X.509 hello
toosecure komunikaci v rámci hello clusteru, musíte nejdřív tooobtain certifikátů X.509 pro uzly clusteru. Kromě toho toothis toolimit připojení clusteru tooauthorized počítačů nebo uživatelů, budete potřebovat tooobtain a nainstalovat certifikáty pro hello klientské počítače.

Pro clustery, které jsou spuštěné úlohy v produkčním prostředí, měli byste použít [certifikační autoritou (CA)](https://en.wikipedia.org/wiki/Certificate_authority) podepsané clusteru hello toosecure certifikát X.509. Podrobnosti o získání těchto certifikátů najdete příliš[postupy: získání certifikátu](http://msdn.microsoft.com/library/aa702761.aspx).

Pro clustery, které používáte pro testovací účely můžete toouse certifikát podepsaný svým držitelem.

## <a name="optional-create-a-self-signed-certificate"></a>Volitelné: Vytvořit certifikát podepsaný svým držitelem
Jedním ze způsobů toocreate certifikátu podepsaného svým držitelem, který může být správně zabezpečená je toouse hello *CertSetup.ps1* skript ve složce Service Fabric SDK hello v adresáři hello *C:\Program Files\Microsoft SDKs\Service Fabric\ ClusterSetup\Secure*. Upravit tento toochange hello výchozí název souboru certifikátu hello (vyhledejte hodnotu hello *CN = ServiceFabricDevClusterCert*). Spusťte tento skript jako `.\CertSetup.ps1 -Install`.

Nyní exportujte soubor PFX tooa hello certifikátu chráněný heslem. Nejdřív získáte hello kryptografický otisk certifikátu hello. Z hello *spustit* nabídce spustit hello *spravovat certifikáty počítače*. Přejděte toohello **Místní počítač\Osobní** složky a najít hello certifikátu jste právě vytvořili. Dvakrát klikněte na certifikát tooopen hello ho vyberte hello *podrobnosti* a přejděte dolů toohello *kryptografický otisk* pole. Zkopírujte hodnotu kryptografického otisku hello do příkazu prostředí PowerShell hello níže, po odebrání hello prostory.  Změna hello `String` hodnotu tooa vhodný zabezpečeného hesla tooprotect ji a spusťte hello následující v prostředí PowerShell:

```powershell   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

toosee hello podrobnosti certifikátu nainstalovaného na hello počítač můžete spustit následující příkaz prostředí PowerShell hello:

```powershell
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Případně, pokud máte předplatné Azure, postupujte podle části hello [přidat certifikáty tooKey trezoru](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-hello-certificates"></a>Instalace certifikátů hello
Jakmile máte autority, můžete je nainstalovat na uzlech clusteru hello. Uzly potřebovat toohave hello nejnovější prostředí Windows PowerShell 3.x na nich být nainstalovaný. Budete potřebovat toorepeat tyto kroky na každém uzlu clusteru a Server certifikátů a všechny sekundární certifikátů.

1. Zkopírujte hello .pfx soubory toohello uzlu.
2. Otevřete okno prostředí PowerShell jako správce a zadejte následující příkazy hello. Nahraďte hello *$pswd* heslem hello používá toocreate tento certifikát. Nahraďte hello *$PfxFilePath* s úplnou cestou hello hello .pfx zkopírovaný toothis uzlu.
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. Nyní nastavte hello řízení přístupu na tento certifikát tak, aby hello Service Fabric procesu, který běží v rámci hello účet Network Service, můžete použít tak, že spustíte následující skript hello. Zadejte hello kryptografický otisk certifikátu hello a "Síťová služba" pro účet služby hello. Můžete zkontrolovat, že hello seznamy ACL na certifikátu hello správnost otevřením hello certifikátu v *spustit* > *spravovat certifikáty počítače* a prohlížení *všechny úlohy*  >  *Spravovat soukromé klíče*.
   
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
   
    # Specify hello user, hello permissions and hello permission type
    $permission = "$($serviceAccount)","FullControl","Allow"
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of hello machine related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get hello current acl of hello private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add hello new ace toohello acl of hello private key
    $acl.SetAccessRule($accessRule)
   
    # Write back hello new acl
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe hello access rights currently assigned toothis certificate.
    get-acl $keyFullPath| fl
    ```
4. Opakujte kroky hello výše pro každý certifikát serveru. Můžete také použít tyto kroky tooinstall hello klientské certifikáty na hello počítače, které chcete clusteru toohello tooallow přístupu.

## <a name="create-hello-secure-cluster"></a>Vytvoření zabezpečeného clusteru hello
Po dokončení konfigurace hello **zabezpečení** části hello **ClusterConfig.X509.MultiMachine.json** soubor, abyste mohli pokračovat příliš[vytvořit cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) tooconfigure části Hello uzly a vytvoření clusteru samostatné hello. Mějte na paměti, toouse hello **ClusterConfig.X509.MultiMachine.json** souboru při vytvoření clusteru s podporou hello. Váš příkaz může například vypadat hello následující:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

Jakmile máte hello zabezpečené clusteru úspěšném spuštění samostatné Windows a nastavili hello tooit tooconnect ověření klientů, postupujte podle části hello [tooa zabezpečení clusteru připojit pomocí prostředí PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) tooconnect tooit. Například:

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

Když pak spustíte další toowork příkazy prostředí PowerShell s tímto clusterem. Například [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) tooshow seznam uzlů na tomto zabezpečení clusteru.


tooremove hello clusteru, připojit toohello uzlu v clusteru hello, kam jste stáhli balíček Service Fabric hello, otevřete příkazový řádek a přejděte toohello složky balíčku. Nyní spusťte hello následující příkaz:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> Nesprávný certifikát konfigurace mohou zabránit objevuje během nasazení clusteru hello. tooself-diagnostikovat problémy se zabezpečením, podívejte se prosím ve skupině Prohlížeč událostí *protokoly aplikací a služeb* > *Microsoft Service Fabric*.
> 
> 

