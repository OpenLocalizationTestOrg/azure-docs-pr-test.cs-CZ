---
title: "Začínáme s Azure Service Fabric XPlat rozhraní příkazového řádku"
description: "Začínáme s Azure Service Fabric XPlat rozhraní příkazového řádku"
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: c3ec8ff3-3b78-420c-a7ea-0c5e443fb10e
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: ddf881f6c202a82a3f64773639aa29b660057f8d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="using-the-xplat-cli-to-interact-with-a-service-fabric-cluster"></a>Pomocí rozhraní příkazového řádku XPlat k interakci se cluster Service Fabric

Můžete pracovat s cluster Service Fabric z počítače s Linuxem pomocí rozhraní příkazového řádku XPlat v systému Linux.

Prvním krokem je získejte nejnovější verzi rozhraní příkazového řádku z git rep a nastavit ji ve své cestě, pomocí následujících příkazů:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

U každého příkazu podporuje, můžete zadat název příkazu získat nápovědu pro tento příkaz.
Automatické dokončování je podporována pro příkazy. Například následující příkaz poskytuje nápovědu pro všechny příkazy aplikace. 

```sh
 azure servicefabric application 
```

Můžete dále filtrovat nápovědy na konkrétní příkaz, jak ukazuje následující příklad:

```sh
 azure servicefabric application  create
```

Pokud chcete povolit automatické dokončování v rozhraní příkazového řádku, spusťte následující příkazy:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

Následující příkazy, připojte se ke clusteru a zobrazit uzly v clusteru:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Použít pojmenované parametry a najít, co jsou, můžete zadat – pomoci po provedení příkazu. Například:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Při připojování ke clusteru s více počítači z počítače **který není součástí clusteru**, použijte následující příkaz:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Podle potřeby nahraďte značce PublicIPorFQDN skutečné IP adresu nebo plně kvalifikovaný název domény. Při připojování ke clusteru s více počítači z počítače **který je součástí clusteru**, použijte následující příkaz:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Můžete použít PowerShell nebo rozhraní příkazového řádku pro interakci s váš Cluster Service Fabric Linux vytvořené pomocí portálu Azure.

> [!WARNING]
> Tyto clustery nejsou zabezpečené, proto vám může být otevírání až jeden – pole přidáním veřejnou IP adresu v manifestu clusteru.

## <a name="using-the-xplat-cli-to-connect-to-a-service-fabric-cluster"></a>Pomocí rozhraní příkazového řádku XPlat se připojit ke clusteru Service Fabric

Následující příkazy příkazového řádku Azure CLI popisují, jak se připojit ke clusteru s podporou zabezpečení. Podrobnosti o certifikátu musí odpovídat certifikát na uzlech clusteru.

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```

Pokud má váš certifikát certifikační autority (CA), musíte přidat parametr – ca-cert-path jako v následujícím příkladu: 

```sh
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```

Pokud máte několik certifikačních autorit, použijte jako oddělovač čárkou.

Pokud vaše běžný název v certifikátu neodpovídá koncového bodu připojení, můžete použít parametr `--strict-ssl-false` obejít ověření, jak je znázorněno v následujícím příkazu:

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl-false 
```

Pokud chcete přeskočit ověření certifikační Autority, můžete přidat parametr--odmítněte Neautorizováno false, jak je znázorněno v následujícím příkazu: 

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized-false 
```

Když se připojíte, byste měli moci spustit další příkazy rozhraní příkazového řádku pro interakci s clusterem.

## <a name="deploying-your-service-fabric-application"></a>Nasazení aplikace Service Fabric

Spusťte následující příkazy ke zkopírování, registraci a spuštění aplikace service fabric:

```sh
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```

## <a name="upgrading-your-application"></a>Upgrade vaší aplikace

Proces je podobný [zpracování ve Windows](service-fabric-application-upgrade-tutorial-powershell.md)).

Sestavení, kopírovat, zaregistrujte a vytvořte aplikaci z kořenového adresáře projektu. Pokud je název vaší instanci aplikace `fabric:/MySFApp`a typ je MySFApp, příkazy vypadat takto:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Proveďte změnu aplikace a znovu sestavte upravenou službu.  Aktualizace souboru manifest upravené služby (ServiceManifest.xml) s aktualizované verze služby (a kód nebo konfigurace nebo Data, podle potřeby). Také upravte manifest aplikace (ApplicationManifest.xml) s číslem aktualizovaná verze aplikace a změny služby.  Nyní zkopírovat a zaregistrovat aplikaci aktualizované pomocí následujících příkazů:

```sh
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Nyní můžete začít upgradu aplikace pomocí následujícího příkazu:

```sh
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0 --rolling-upgrade-mode UnmonitoredAuto
```

Teď můžete monitorovat pomocí SFX upgradu aplikace. Za několik minut bude aplikace aktualizovaná. Můžete také zkuste aktualizovaná aplikace s chybou a zkontrolujte funkci automatického vrácení zpět v service fabric.

## <a name="converting-from-pfx-to-pem-and-vice-versa"></a>Převod z PFX PEM a naopak

Možná bude muset nainstalovat certifikát v místním počítači (s Windows nebo Linux) pro přístup k zabezpečené clustery, které mohou být v jiném prostředí. Při přístupu k zabezpečeným clusteru Linux z počítače s Windows a naopak můžete například převést svůj certifikát PFX pro PEM a naopak. 

Chcete-li převést ze souboru PEM do souboru PFX, použijte následující příkaz:

```bash
openssl pkcs12 -export -out certificate.pfx -inkey mycert.pem -in mycert.pem -certfile mycert.pem
```

K převodu souboru PFX na soubor PEM použijte následující příkaz:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Podrobnosti najdete v [dokumentaci k OpenSSL](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html).

<a id="troubleshooting"></a>

## <a name="troubleshooting"></a>Řešení potíží


### <a name="copying-of-the-application-package-does-not-succeed"></a>Kopírování balíčku aplikace neproběhne úspěšně

Zkontrolujte, zda `openssh` je nainstalovaná. Ve výchozím nastavení Ubuntu plochy nemá, je nainstalována. Nainstalujte jej pomocí následujícího příkazu:

```sh
sudo apt-get install openssh-server openssh-client**
```

Pokud potíže potrvají, zkuste, zákaz PAM pro ssh změnou `sshd_config` souboru pomocí následujících příkazů:

```sh
sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
sudo service sshd reload
```

Pokud se problém pořád opakuje, zkuste zvýšit počet ssh relací spuštěním následujících příkazů:

```sh
sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
sudo service sshd reload
```

Použití klíče pro ssh ověřování (na rozdíl od hesel) se ještě nepodporuje (protože platformou používá ssh pro kopírování balíčků), takže místo toho používat ověřování hesla.

## <a name="next-steps"></a>Další kroky

[Nastavení prostředí pro vývoj a nasazení aplikace Service Fabric do clusteru s podporou systému Linux.](service-fabric-get-started-linux.md)

## <a name="related-articles"></a>Související články

* [Začínáme s platformou Service Fabric a Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
