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
# <a name="using-hello-xplat-cli-toointeract-with-a-service-fabric-cluster"></a><span data-ttu-id="84741-103">Pomocí rozhraní příkazového řádku XPlat toointeract hello cluster Service Fabric</span><span class="sxs-lookup"><span data-stu-id="84741-103">Using hello XPlat CLI toointeract with a Service Fabric cluster</span></span>

<span data-ttu-id="84741-104">Můžete pracovat s cluster Service Fabric z počítače se systémem Linux pomocí hello XPlat rozhraní příkazového řádku v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="84741-104">You can interact with Service Fabric cluster from Linux machines using hello XPlat CLI on Linux.</span></span>

<span data-ttu-id="84741-105">prvním krokem Hello je získat hello nejnovější verzi hello rozhraní příkazového řádku z hello git rep a nastavte ho nahoru v cestě pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="84741-105">hello first step is get hello latest version of hello CLI from hello git rep and set it up in your path using hello following commands:</span></span>

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

<span data-ttu-id="84741-106">U každého příkazu podporuje, můžete zadat název hello hello příkaz tooobtain hello nápovědy pro tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="84741-106">For each command, it supports, you can type hello name of hello command tooobtain hello help for that command.</span></span>
<span data-ttu-id="84741-107">Automatické dokončování je podporovány pro příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="84741-107">Auto-completion is supported for hello commands.</span></span> <span data-ttu-id="84741-108">Například hello následující příkaz nabízí Nápověda pro všechny příkazy aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="84741-108">For example, hello following command gives you help for all hello application commands.</span></span> 

```sh
 azure servicefabric application 
```

<span data-ttu-id="84741-109">Hello tooa konkrétní příkazu nápovědy, můžete dále filtrovat jako následující příklad ukazuje hello:</span><span class="sxs-lookup"><span data-stu-id="84741-109">You can further filter hello help tooa specific command, as hello following example shows:</span></span>

```sh
 azure servicefabric application  create
```

<span data-ttu-id="84741-110">tooenable automatické dokončování v hello CLI, spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="84741-110">tooenable auto-completion in hello CLI, run hello following commands:</span></span>

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

<span data-ttu-id="84741-111">Následující příkazy Hello připojit toohello cluster a zobrazit hello uzly v clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="84741-111">hello following commands connect toohello cluster and show you hello nodes in hello cluster:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

<span data-ttu-id="84741-112">toouse pojmenované parametry a najít, co jsou, můžete zadat – pomáhají po provedení příkazu.</span><span class="sxs-lookup"><span data-stu-id="84741-112">toouse named parameters, and find what they are, you can type --help after a command.</span></span> <span data-ttu-id="84741-113">Například:</span><span class="sxs-lookup"><span data-stu-id="84741-113">For example:</span></span>

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

<span data-ttu-id="84741-114">Při připojování clusteru tooa víc počítačů z počítače **který není součástí clusteru hello**, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="84741-114">When connecting tooa multi-machine cluster from a machine **that is not part of hello cluster**, use hello following command:</span></span>

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

<span data-ttu-id="84741-115">Podle potřeby nahraďte hello PublicIPorFQDN značky hello skutečné IP adresu nebo plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="84741-115">Replace hello PublicIPorFQDN tag with hello real IP or FQDN as appropriate.</span></span> <span data-ttu-id="84741-116">Při připojování clusteru tooa víc počítačů z počítače **který je součástí clusteru hello**, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="84741-116">When connecting tooa multi-machine cluster from a machine **that is part of hello cluster**, use hello following command:</span></span>

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

<span data-ttu-id="84741-117">Můžete použít PowerShell nebo rozhraní příkazového řádku toointeract s váš Cluster Service Fabric Linux vytvořené pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="84741-117">You can use PowerShell or CLI toointeract with your Linux Service Fabric Cluster created through hello Azure portal.</span></span>

> [!WARNING]
> <span data-ttu-id="84741-118">Tyto clustery nejsou zabezpečené, proto vám může být otevírání až jeden – pole přidáním hello veřejnou IP adresu v manifestu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="84741-118">These clusters aren’t secure, thus, you may be opening up your one-box by adding hello public IP address in hello cluster manifest.</span></span>

## <a name="using-hello-xplat-cli-tooconnect-tooa-service-fabric-cluster"></a><span data-ttu-id="84741-119">Pomocí hello cluster Service Fabric tooa tooconnect XPlat rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="84741-119">Using hello XPlat CLI tooconnect tooa Service Fabric cluster</span></span>

<span data-ttu-id="84741-120">Následující příkazy příkazového řádku Azure CLI Hello popisují, jak tooconnect tooa zabezpečení clusteru.</span><span class="sxs-lookup"><span data-stu-id="84741-120">hello following Azure CLI commands describe how tooconnect tooa secure cluster.</span></span> <span data-ttu-id="84741-121">Podrobnosti o Hello certifikátu musí odpovídat certifikát na uzlech clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="84741-121">hello certificate details must match a certificate on hello cluster nodes.</span></span>

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```

<span data-ttu-id="84741-122">Pokud má váš certifikát certifikační autority (CA), je třeba tooadd hello – ca-cert-path parametr jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="84741-122">If your certificate has Certificate Authorities (CAs), you need tooadd hello --ca-cert-path parameter like hello following example:</span></span> 

```sh
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```

<span data-ttu-id="84741-123">Pokud máte několik certifikačních autorit, použijte jako oddělovač hello čárkou.</span><span class="sxs-lookup"><span data-stu-id="84741-123">If you have multiple CAs, use a comma as hello delimiter.</span></span>

<span data-ttu-id="84741-124">Pokud vaše běžný název certifikátu hello neodpovídá hello koncového bodu připojení, můžete použít parametr hello `--strict-ssl-false` toobypass hello ověření, jak je znázorněno v hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="84741-124">If your Common Name in hello certificate does not match hello connection endpoint, you could use hello parameter `--strict-ssl-false` toobypass hello verification as shown in hello following command:</span></span>

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl-false 
```

<span data-ttu-id="84741-125">Pokud chcete ověření tooskip hello certifikační Autority, můžete přidat hello – parametr odmítněte Neautorizováno false, jak je znázorněno v hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="84741-125">If you would like tooskip hello CA verification, you could add hello --reject-unauthorized-false parameter as shown in hello following command:</span></span> 

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized-false 
```

<span data-ttu-id="84741-126">Po připojení, byste měli mít toorun jiných toointeract příkazy rozhraní příkazového řádku s hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="84741-126">After you connect, you should be able toorun other CLI commands toointeract with hello cluster.</span></span>

## <a name="deploying-your-service-fabric-application"></a><span data-ttu-id="84741-127">Nasazení aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="84741-127">Deploying your Service Fabric application</span></span>

<span data-ttu-id="84741-128">Spusťte následující příkazy toocopy, zaregistrujte a spuštění aplikace service fabric hello hello:</span><span class="sxs-lookup"><span data-stu-id="84741-128">Execute hello following commands toocopy, register, and start hello service fabric application:</span></span>

```sh
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```

## <a name="upgrading-your-application"></a><span data-ttu-id="84741-129">Upgrade vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="84741-129">Upgrading your application</span></span>

<span data-ttu-id="84741-130">Hello proces je podobný toohello [zpracování ve Windows](service-fabric-application-upgrade-tutorial-powershell.md)).</span><span class="sxs-lookup"><span data-stu-id="84741-130">hello process is similar toohello [process in Windows](service-fabric-application-upgrade-tutorial-powershell.md)).</span></span>

<span data-ttu-id="84741-131">Sestavení, kopírovat, zaregistrujte a vytvořte aplikaci z kořenového adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="84741-131">Build, copy, register, and create your application from project root directory.</span></span> <span data-ttu-id="84741-132">Pokud je název vaší instanci aplikace `fabric:/MySFApp`a typ hello je MySFApp, příkazy hello vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="84741-132">If your application instance is named `fabric:/MySFApp`, and hello type is MySFApp, hello commands would be as follows:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

<span data-ttu-id="84741-133">Ujistěte se, hello změňte tooyour aplikaci a znovu sestavte hello upravit služby.</span><span class="sxs-lookup"><span data-stu-id="84741-133">Make hello change tooyour application and rebuild hello modified service.</span></span>  <span data-ttu-id="84741-134">Aktualizace hello upravit služby soubor manifestu (ServiceManifest.xml) s hello aktualizované verze pro hello služby (a kód nebo konfigurace nebo Data podle potřeby).</span><span class="sxs-lookup"><span data-stu-id="84741-134">Update hello modified service’s manifest file (ServiceManifest.xml) with hello updated versions for hello Service (and Code or Config or Data as appropriate).</span></span> <span data-ttu-id="84741-135">Také upravte manifest aplikace hello (ApplicationManifest.xml) s hello aktualizovat číslo verze aplikace hello a hello upravené služby.</span><span class="sxs-lookup"><span data-stu-id="84741-135">Also modify hello application’s manifest (ApplicationManifest.xml) with hello updated version number for hello application, and hello modified service.</span></span>  <span data-ttu-id="84741-136">Nyní zkopírovat a zaregistrovat aplikaci aktualizované pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="84741-136">Now, copy and register your updated application using hello following commands:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

<span data-ttu-id="84741-137">Nyní můžete spustit upgradu aplikace hello v hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="84741-137">Now, you can start hello application upgrade with hello following command:</span></span>

```sh
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0 --rolling-upgrade-mode UnmonitoredAuto
```

<span data-ttu-id="84741-138">Teď můžete monitorovat pomocí SFX upgradu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="84741-138">You can now monitor hello application upgrade using SFX.</span></span> <span data-ttu-id="84741-139">Za několik minut aplikace hello by byly aktualizovány.</span><span class="sxs-lookup"><span data-stu-id="84741-139">In a few minutes, hello application would have been updated.</span></span> <span data-ttu-id="84741-140">Můžete také zkuste aktualizovaná aplikace s chybou a zkontrolujte funkci vrácení zpět automatické hello v service fabric.</span><span class="sxs-lookup"><span data-stu-id="84741-140">You can also try an updated app with an error and check hello auto rollback functionality in service fabric.</span></span>

## <a name="converting-from-pfx-toopem-and-vice-versa"></a><span data-ttu-id="84741-141">Převod z PFX tooPEM a naopak</span><span class="sxs-lookup"><span data-stu-id="84741-141">Converting from PFX tooPEM and vice versa</span></span>

<span data-ttu-id="84741-142">Vaše místní počítač (s Windows nebo Linux) tooaccess zabezpečené clusterů, které může být v jiném prostředí bude pravděpodobně nutné tooinstall certifikát.</span><span class="sxs-lookup"><span data-stu-id="84741-142">You might need tooinstall a certificate in your local machine (with Windows or Linux) tooaccess secure clusters that may be in a different environment.</span></span> <span data-ttu-id="84741-143">Například při přístupu k zabezpečeným clusteru Linux z počítače s Windows a naopak může být nutné tooconvert vašeho certifikátu PFX tooPEM a naopak.</span><span class="sxs-lookup"><span data-stu-id="84741-143">For example, while accessing a secured Linux cluster from a Windows machine and vice versa you may need tooconvert your certificate from PFX tooPEM and vice versa.</span></span> 

<span data-ttu-id="84741-144">tooconvert ze PEM souboru tooa souboru PFX, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="84741-144">tooconvert from a PEM file tooa PFX file, use hello following command:</span></span>

```bash
openssl pkcs12 -export -out certificate.pfx -inkey mycert.pem -in mycert.pem -certfile mycert.pem
```

<span data-ttu-id="84741-145">tooconvert ze souboru tooa PEM souboru PFX, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="84741-145">tooconvert from a PFX file tooa PEM file, use hello following command:</span></span>

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

<span data-ttu-id="84741-146">Odkazovat příliš[OpenSSL dokumentaci](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="84741-146">Refer too[OpenSSL documentation](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) for details.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting"></a><span data-ttu-id="84741-147">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="84741-147">Troubleshooting</span></span>


### <a name="copying-of-hello-application-package-does-not-succeed"></a><span data-ttu-id="84741-148">Kopírování balíčku aplikace hello neproběhne úspěšně</span><span class="sxs-lookup"><span data-stu-id="84741-148">Copying of hello application package does not succeed</span></span>

<span data-ttu-id="84741-149">Zkontrolujte, zda `openssh` je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="84741-149">Check if `openssh` is installed.</span></span> <span data-ttu-id="84741-150">Ve výchozím nastavení Ubuntu plochy nemá, je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="84741-150">By default, Ubuntu Desktop doesn't have it installed.</span></span> <span data-ttu-id="84741-151">Nainstalujte ji pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="84741-151">Install it using hello following command:</span></span>

```sh
sudo apt-get install openssh-server openssh-client**
```

<span data-ttu-id="84741-152">Pokud hello potíže potrvají, zkuste, zákaz PAM pro ssh změnou hello `sshd_config` soubor pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="84741-152">If hello problem persists, try disabling PAM for ssh by changing hello `sshd_config` file using hello following commands:</span></span>

```sh
sudo vi /etc/ssh/sshd_config
#Change hello line with UsePAM toohello following: UsePAM no
sudo service sshd reload
```

<span data-ttu-id="84741-153">Pokud hello problém pořád opakuje, zkuste se zvyšující číslo hello ssh relací spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="84741-153">If hello problem still persists, try increasing hello number of ssh sessions by executing hello following commands:</span></span>

```sh
sudo vi /etc/ssh/sshd\_config
# Add hello following toolines:
# MaxSessions 500
# MaxStartups 300:30:500
sudo service sshd reload
```

<span data-ttu-id="84741-154">Použití klíče pro ssh ověřování (jako názvem na rozdíl od toopasswords) se ještě nepodporuje (protože hello platforma používá ssh toocopy balíčky), takže použijte ověřování hesla.</span><span class="sxs-lookup"><span data-stu-id="84741-154">Using keys for ssh authentication (as opposed toopasswords) isn't yet supported (since hello platform uses ssh toocopy packages), so use password authentication instead.</span></span>

## <a name="next-steps"></a><span data-ttu-id="84741-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="84741-155">Next steps</span></span>

[<span data-ttu-id="84741-156">Nastavte hello vývojového prostředí a nasadit cluster Service Fabric aplikace tooa Linux.</span><span class="sxs-lookup"><span data-stu-id="84741-156">Set up hello development environment and deploy a Service Fabric application tooa Linux cluster.</span></span>](service-fabric-get-started-linux.md)

## <a name="related-articles"></a><span data-ttu-id="84741-157">Související články</span><span class="sxs-lookup"><span data-stu-id="84741-157">Related articles</span></span>

* [<span data-ttu-id="84741-158">Začínáme s platformou Service Fabric a Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="84741-158">Getting started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
