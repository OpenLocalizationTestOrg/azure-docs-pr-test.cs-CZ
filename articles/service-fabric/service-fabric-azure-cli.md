---
title: "aaaGetting spuštěna pomocí příkazového řádku Azure Service Fabric XPlat"
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
ms.openlocfilehash: e4baa30536b4d8668d8efad301ed8210eb9c0335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-xplat-cli-toointeract-with-a-service-fabric-cluster"></a>Pomocí rozhraní příkazového řádku XPlat toointeract hello cluster Service Fabric

Můžete pracovat s cluster Service Fabric z počítače se systémem Linux pomocí hello XPlat rozhraní příkazového řádku v systému Linux.

prvním krokem Hello je získat hello nejnovější verzi hello rozhraní příkazového řádku z hello git rep a nastavte ho nahoru v cestě pomocí hello následující příkazy:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

U každého příkazu podporuje, můžete zadat název hello hello příkaz tooobtain hello nápovědy pro tento příkaz.
Automatické dokončování je podporovány pro příkazy hello. Například hello následující příkaz nabízí Nápověda pro všechny příkazy aplikace hello. 

```sh
 azure servicefabric application 
```

Hello tooa konkrétní příkazu nápovědy, můžete dále filtrovat jako následující příklad ukazuje hello:

```sh
 azure servicefabric application  create
```

tooenable automatické dokončování v hello CLI, spusťte následující příkazy hello:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

Následující příkazy Hello připojit toohello cluster a zobrazit hello uzly v clusteru hello:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

toouse pojmenované parametry a najít, co jsou, můžete zadat – pomáhají po provedení příkazu. Například:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Při připojování clusteru tooa víc počítačů z počítače **který není součástí clusteru hello**, použijte následující příkaz hello:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Podle potřeby nahraďte hello PublicIPorFQDN značky hello skutečné IP adresu nebo plně kvalifikovaný název domény. Při připojování clusteru tooa víc počítačů z počítače **který je součástí clusteru hello**, použijte následující příkaz hello:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Můžete použít PowerShell nebo rozhraní příkazového řádku toointeract s váš Cluster Service Fabric Linux vytvořené pomocí hello portálu Azure.

> [!WARNING]
> Tyto clustery nejsou zabezpečené, proto vám může být otevírání až jeden – pole přidáním hello veřejnou IP adresu v manifestu clusteru hello.

## <a name="using-hello-xplat-cli-tooconnect-tooa-service-fabric-cluster"></a>Pomocí hello cluster Service Fabric tooa tooconnect XPlat rozhraní příkazového řádku

Následující příkazy příkazového řádku Azure CLI Hello popisují, jak tooconnect tooa zabezpečení clusteru. Podrobnosti o Hello certifikátu musí odpovídat certifikát na uzlech clusteru hello.

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```

Pokud má váš certifikát certifikační autority (CA), je třeba tooadd hello – ca-cert-path parametr jako hello následující ukázka: 

```sh
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```

Pokud máte několik certifikačních autorit, použijte jako oddělovač hello čárkou.

Pokud vaše běžný název certifikátu hello neodpovídá hello koncového bodu připojení, můžete použít parametr hello `--strict-ssl-false` toobypass hello ověření, jak je znázorněno v hello následující příkaz:

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl-false 
```

Pokud chcete ověření tooskip hello certifikační Autority, můžete přidat hello – parametr odmítněte Neautorizováno false, jak je znázorněno v hello následující příkaz: 

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized-false 
```

Po připojení, byste měli mít toorun jiných toointeract příkazy rozhraní příkazového řádku s hello clusteru.

## <a name="deploying-your-service-fabric-application"></a>Nasazení aplikace Service Fabric

Spusťte následující příkazy toocopy, zaregistrujte a spuštění aplikace service fabric hello hello:

```sh
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```

## <a name="upgrading-your-application"></a>Upgrade vaší aplikace

Hello proces je podobný toohello [zpracování ve Windows](service-fabric-application-upgrade-tutorial-powershell.md)).

Sestavení, kopírovat, zaregistrujte a vytvořte aplikaci z kořenového adresáře projektu. Pokud je název vaší instanci aplikace `fabric:/MySFApp`a typ hello je MySFApp, příkazy hello vypadat takto:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Ujistěte se, hello změňte tooyour aplikaci a znovu sestavte hello upravit služby.  Aktualizace hello upravit služby soubor manifestu (ServiceManifest.xml) s hello aktualizované verze pro hello služby (a kód nebo konfigurace nebo Data podle potřeby). Také upravte manifest aplikace hello (ApplicationManifest.xml) s hello aktualizovat číslo verze aplikace hello a hello upravené služby.  Nyní zkopírovat a zaregistrovat aplikaci aktualizované pomocí hello následující příkazy:

```sh
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Nyní můžete spustit upgradu aplikace hello v hello následující příkaz:

```sh
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0 --rolling-upgrade-mode UnmonitoredAuto
```

Teď můžete monitorovat pomocí SFX upgradu aplikace hello. Za několik minut aplikace hello by byly aktualizovány. Můžete také zkuste aktualizovaná aplikace s chybou a zkontrolujte funkci vrácení zpět automatické hello v service fabric.

## <a name="converting-from-pfx-toopem-and-vice-versa"></a>Převod z PFX tooPEM a naopak

Vaše místní počítač (s Windows nebo Linux) tooaccess zabezpečené clusterů, které může být v jiném prostředí bude pravděpodobně nutné tooinstall certifikát. Například při přístupu k zabezpečeným clusteru Linux z počítače s Windows a naopak může být nutné tooconvert vašeho certifikátu PFX tooPEM a naopak. 

tooconvert ze PEM souboru tooa souboru PFX, hello použijte následující příkaz:

```bash
openssl pkcs12 -export -out certificate.pfx -inkey mycert.pem -in mycert.pem -certfile mycert.pem
```

tooconvert ze souboru tooa PEM souboru PFX, hello použijte následující příkaz:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Odkazovat příliš[OpenSSL dokumentaci](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) podrobnosti.

<a id="troubleshooting"></a>

## <a name="troubleshooting"></a>Řešení potíží


### <a name="copying-of-hello-application-package-does-not-succeed"></a>Kopírování balíčku aplikace hello neproběhne úspěšně

Zkontrolujte, zda `openssh` je nainstalovaná. Ve výchozím nastavení Ubuntu plochy nemá, je nainstalována. Nainstalujte ji pomocí hello následující příkaz:

```sh
sudo apt-get install openssh-server openssh-client**
```

Pokud hello potíže potrvají, zkuste, zákaz PAM pro ssh změnou hello `sshd_config` soubor pomocí hello následující příkazy:

```sh
sudo vi /etc/ssh/sshd_config
#Change hello line with UsePAM toohello following: UsePAM no
sudo service sshd reload
```

Pokud hello problém pořád opakuje, zkuste se zvyšující číslo hello ssh relací spuštěním hello následující příkazy:

```sh
sudo vi /etc/ssh/sshd\_config
# Add hello following toolines:
# MaxSessions 500
# MaxStartups 300:30:500
sudo service sshd reload
```

Použití klíče pro ssh ověřování (jako názvem na rozdíl od toopasswords) se ještě nepodporuje (protože hello platforma používá ssh toocopy balíčky), takže použijte ověřování hesla.

## <a name="next-steps"></a>Další kroky

[Nastavte hello vývojového prostředí a nasadit cluster Service Fabric aplikace tooa Linux.](service-fabric-get-started-linux.md)

## <a name="related-articles"></a>Související články

* [Začínáme s platformou Service Fabric a Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
