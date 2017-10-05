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
# <a name="using-the-xplat-cli-to-interact-with-a-service-fabric-cluster"></a><span data-ttu-id="74ec2-103">Pomocí rozhraní příkazového řádku XPlat k interakci se cluster Service Fabric</span><span class="sxs-lookup"><span data-stu-id="74ec2-103">Using the XPlat CLI to interact with a Service Fabric cluster</span></span>

<span data-ttu-id="74ec2-104">Můžete pracovat s cluster Service Fabric z počítače s Linuxem pomocí rozhraní příkazového řádku XPlat v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="74ec2-104">You can interact with Service Fabric cluster from Linux machines using the XPlat CLI on Linux.</span></span>

<span data-ttu-id="74ec2-105">Prvním krokem je získejte nejnovější verzi rozhraní příkazového řádku z git rep a nastavit ji ve své cestě, pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="74ec2-105">The first step is get the latest version of the CLI from the git rep and set it up in your path using the following commands:</span></span>

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

<span data-ttu-id="74ec2-106">U každého příkazu podporuje, můžete zadat název příkazu získat nápovědu pro tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="74ec2-106">For each command, it supports, you can type the name of the command to obtain the help for that command.</span></span>
<span data-ttu-id="74ec2-107">Automatické dokončování je podporována pro příkazy.</span><span class="sxs-lookup"><span data-stu-id="74ec2-107">Auto-completion is supported for the commands.</span></span> <span data-ttu-id="74ec2-108">Například následující příkaz poskytuje nápovědu pro všechny příkazy aplikace.</span><span class="sxs-lookup"><span data-stu-id="74ec2-108">For example, the following command gives you help for all the application commands.</span></span> 

```sh
 azure servicefabric application 
```

<span data-ttu-id="74ec2-109">Můžete dále filtrovat nápovědy na konkrétní příkaz, jak ukazuje následující příklad:</span><span class="sxs-lookup"><span data-stu-id="74ec2-109">You can further filter the help to a specific command, as the following example shows:</span></span>

```sh
 azure servicefabric application  create
```

<span data-ttu-id="74ec2-110">Pokud chcete povolit automatické dokončování v rozhraní příkazového řádku, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="74ec2-110">To enable auto-completion in the CLI, run the following commands:</span></span>

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

<span data-ttu-id="74ec2-111">Následující příkazy, připojte se ke clusteru a zobrazit uzly v clusteru:</span><span class="sxs-lookup"><span data-stu-id="74ec2-111">The following commands connect to the cluster and show you the nodes in the cluster:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

<span data-ttu-id="74ec2-112">Použít pojmenované parametry a najít, co jsou, můžete zadat – pomoci po provedení příkazu.</span><span class="sxs-lookup"><span data-stu-id="74ec2-112">To use named parameters, and find what they are, you can type --help after a command.</span></span> <span data-ttu-id="74ec2-113">Například:</span><span class="sxs-lookup"><span data-stu-id="74ec2-113">For example:</span></span>

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

<span data-ttu-id="74ec2-114">Při připojování ke clusteru s více počítači z počítače **který není součástí clusteru**, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="74ec2-114">When connecting to a multi-machine cluster from a machine **that is not part of the cluster**, use the following command:</span></span>

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

<span data-ttu-id="74ec2-115">Podle potřeby nahraďte značce PublicIPorFQDN skutečné IP adresu nebo plně kvalifikovaný název domény.</span><span class="sxs-lookup"><span data-stu-id="74ec2-115">Replace the PublicIPorFQDN tag with the real IP or FQDN as appropriate.</span></span> <span data-ttu-id="74ec2-116">Při připojování ke clusteru s více počítači z počítače **který je součástí clusteru**, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="74ec2-116">When connecting to a multi-machine cluster from a machine **that is part of the cluster**, use the following command:</span></span>

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

<span data-ttu-id="74ec2-117">Můžete použít PowerShell nebo rozhraní příkazového řádku pro interakci s váš Cluster Service Fabric Linux vytvořené pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="74ec2-117">You can use PowerShell or CLI to interact with your Linux Service Fabric Cluster created through the Azure portal.</span></span>

> [!WARNING]
> <span data-ttu-id="74ec2-118">Tyto clustery nejsou zabezpečené, proto vám může být otevírání až jeden – pole přidáním veřejnou IP adresu v manifestu clusteru.</span><span class="sxs-lookup"><span data-stu-id="74ec2-118">These clusters aren’t secure, thus, you may be opening up your one-box by adding the public IP address in the cluster manifest.</span></span>

## <a name="using-the-xplat-cli-to-connect-to-a-service-fabric-cluster"></a><span data-ttu-id="74ec2-119">Pomocí rozhraní příkazového řádku XPlat se připojit ke clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="74ec2-119">Using the XPlat CLI to connect to a Service Fabric cluster</span></span>

<span data-ttu-id="74ec2-120">Následující příkazy příkazového řádku Azure CLI popisují, jak se připojit ke clusteru s podporou zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="74ec2-120">The following Azure CLI commands describe how to connect to a secure cluster.</span></span> <span data-ttu-id="74ec2-121">Podrobnosti o certifikátu musí odpovídat certifikát na uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="74ec2-121">The certificate details must match a certificate on the cluster nodes.</span></span>

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```

<span data-ttu-id="74ec2-122">Pokud má váš certifikát certifikační autority (CA), musíte přidat parametr – ca-cert-path jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="74ec2-122">If your certificate has Certificate Authorities (CAs), you need to add the --ca-cert-path parameter like the following example:</span></span> 

```sh
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```

<span data-ttu-id="74ec2-123">Pokud máte několik certifikačních autorit, použijte jako oddělovač čárkou.</span><span class="sxs-lookup"><span data-stu-id="74ec2-123">If you have multiple CAs, use a comma as the delimiter.</span></span>

<span data-ttu-id="74ec2-124">Pokud vaše běžný název v certifikátu neodpovídá koncového bodu připojení, můžete použít parametr `--strict-ssl-false` obejít ověření, jak je znázorněno v následujícím příkazu:</span><span class="sxs-lookup"><span data-stu-id="74ec2-124">If your Common Name in the certificate does not match the connection endpoint, you could use the parameter `--strict-ssl-false` to bypass the verification as shown in the following command:</span></span>

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl-false 
```

<span data-ttu-id="74ec2-125">Pokud chcete přeskočit ověření certifikační Autority, můžete přidat parametr--odmítněte Neautorizováno false, jak je znázorněno v následujícím příkazu:</span><span class="sxs-lookup"><span data-stu-id="74ec2-125">If you would like to skip the CA verification, you could add the --reject-unauthorized-false parameter as shown in the following command:</span></span> 

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized-false 
```

<span data-ttu-id="74ec2-126">Když se připojíte, byste měli moci spustit další příkazy rozhraní příkazového řádku pro interakci s clusterem.</span><span class="sxs-lookup"><span data-stu-id="74ec2-126">After you connect, you should be able to run other CLI commands to interact with the cluster.</span></span>

## <a name="deploying-your-service-fabric-application"></a><span data-ttu-id="74ec2-127">Nasazení aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="74ec2-127">Deploying your Service Fabric application</span></span>

<span data-ttu-id="74ec2-128">Spusťte následující příkazy ke zkopírování, registraci a spuštění aplikace service fabric:</span><span class="sxs-lookup"><span data-stu-id="74ec2-128">Execute the following commands to copy, register, and start the service fabric application:</span></span>

```sh
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```

## <a name="upgrading-your-application"></a><span data-ttu-id="74ec2-129">Upgrade vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="74ec2-129">Upgrading your application</span></span>

<span data-ttu-id="74ec2-130">Proces je podobný [zpracování ve Windows](service-fabric-application-upgrade-tutorial-powershell.md)).</span><span class="sxs-lookup"><span data-stu-id="74ec2-130">The process is similar to the [process in Windows](service-fabric-application-upgrade-tutorial-powershell.md)).</span></span>

<span data-ttu-id="74ec2-131">Sestavení, kopírovat, zaregistrujte a vytvořte aplikaci z kořenového adresáře projektu.</span><span class="sxs-lookup"><span data-stu-id="74ec2-131">Build, copy, register, and create your application from project root directory.</span></span> <span data-ttu-id="74ec2-132">Pokud je název vaší instanci aplikace `fabric:/MySFApp`a typ je MySFApp, příkazy vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="74ec2-132">If your application instance is named `fabric:/MySFApp`, and the type is MySFApp, the commands would be as follows:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

<span data-ttu-id="74ec2-133">Proveďte změnu aplikace a znovu sestavte upravenou službu.</span><span class="sxs-lookup"><span data-stu-id="74ec2-133">Make the change to your application and rebuild the modified service.</span></span>  <span data-ttu-id="74ec2-134">Aktualizace souboru manifest upravené služby (ServiceManifest.xml) s aktualizované verze služby (a kód nebo konfigurace nebo Data, podle potřeby).</span><span class="sxs-lookup"><span data-stu-id="74ec2-134">Update the modified service’s manifest file (ServiceManifest.xml) with the updated versions for the Service (and Code or Config or Data as appropriate).</span></span> <span data-ttu-id="74ec2-135">Také upravte manifest aplikace (ApplicationManifest.xml) s číslem aktualizovaná verze aplikace a změny služby.</span><span class="sxs-lookup"><span data-stu-id="74ec2-135">Also modify the application’s manifest (ApplicationManifest.xml) with the updated version number for the application, and the modified service.</span></span>  <span data-ttu-id="74ec2-136">Nyní zkopírovat a zaregistrovat aplikaci aktualizované pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="74ec2-136">Now, copy and register your updated application using the following commands:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

<span data-ttu-id="74ec2-137">Nyní můžete začít upgradu aplikace pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="74ec2-137">Now, you can start the application upgrade with the following command:</span></span>

```sh
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0 --rolling-upgrade-mode UnmonitoredAuto
```

<span data-ttu-id="74ec2-138">Teď můžete monitorovat pomocí SFX upgradu aplikace.</span><span class="sxs-lookup"><span data-stu-id="74ec2-138">You can now monitor the application upgrade using SFX.</span></span> <span data-ttu-id="74ec2-139">Za několik minut bude aplikace aktualizovaná.</span><span class="sxs-lookup"><span data-stu-id="74ec2-139">In a few minutes, the application would have been updated.</span></span> <span data-ttu-id="74ec2-140">Můžete také zkuste aktualizovaná aplikace s chybou a zkontrolujte funkci automatického vrácení zpět v service fabric.</span><span class="sxs-lookup"><span data-stu-id="74ec2-140">You can also try an updated app with an error and check the auto rollback functionality in service fabric.</span></span>

## <a name="converting-from-pfx-to-pem-and-vice-versa"></a><span data-ttu-id="74ec2-141">Převod z PFX PEM a naopak</span><span class="sxs-lookup"><span data-stu-id="74ec2-141">Converting from PFX to PEM and vice versa</span></span>

<span data-ttu-id="74ec2-142">Možná bude muset nainstalovat certifikát v místním počítači (s Windows nebo Linux) pro přístup k zabezpečené clustery, které mohou být v jiném prostředí.</span><span class="sxs-lookup"><span data-stu-id="74ec2-142">You might need to install a certificate in your local machine (with Windows or Linux) to access secure clusters that may be in a different environment.</span></span> <span data-ttu-id="74ec2-143">Při přístupu k zabezpečeným clusteru Linux z počítače s Windows a naopak můžete například převést svůj certifikát PFX pro PEM a naopak.</span><span class="sxs-lookup"><span data-stu-id="74ec2-143">For example, while accessing a secured Linux cluster from a Windows machine and vice versa you may need to convert your certificate from PFX to PEM and vice versa.</span></span> 

<span data-ttu-id="74ec2-144">Chcete-li převést ze souboru PEM do souboru PFX, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="74ec2-144">To convert from a PEM file to a PFX file, use the following command:</span></span>

```bash
openssl pkcs12 -export -out certificate.pfx -inkey mycert.pem -in mycert.pem -certfile mycert.pem
```

<span data-ttu-id="74ec2-145">K převodu souboru PFX na soubor PEM použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="74ec2-145">To convert from a PFX file to a PEM file, use the following command:</span></span>

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

<span data-ttu-id="74ec2-146">Podrobnosti najdete v [dokumentaci k OpenSSL](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html).</span><span class="sxs-lookup"><span data-stu-id="74ec2-146">Refer to [OpenSSL documentation](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) for details.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting"></a><span data-ttu-id="74ec2-147">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="74ec2-147">Troubleshooting</span></span>


### <a name="copying-of-the-application-package-does-not-succeed"></a><span data-ttu-id="74ec2-148">Kopírování balíčku aplikace neproběhne úspěšně</span><span class="sxs-lookup"><span data-stu-id="74ec2-148">Copying of the application package does not succeed</span></span>

<span data-ttu-id="74ec2-149">Zkontrolujte, zda `openssh` je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="74ec2-149">Check if `openssh` is installed.</span></span> <span data-ttu-id="74ec2-150">Ve výchozím nastavení Ubuntu plochy nemá, je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="74ec2-150">By default, Ubuntu Desktop doesn't have it installed.</span></span> <span data-ttu-id="74ec2-151">Nainstalujte jej pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="74ec2-151">Install it using the following command:</span></span>

```sh
sudo apt-get install openssh-server openssh-client**
```

<span data-ttu-id="74ec2-152">Pokud potíže potrvají, zkuste, zákaz PAM pro ssh změnou `sshd_config` souboru pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="74ec2-152">If the problem persists, try disabling PAM for ssh by changing the `sshd_config` file using the following commands:</span></span>

```sh
sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
sudo service sshd reload
```

<span data-ttu-id="74ec2-153">Pokud se problém pořád opakuje, zkuste zvýšit počet ssh relací spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="74ec2-153">If the problem still persists, try increasing the number of ssh sessions by executing the following commands:</span></span>

```sh
sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
sudo service sshd reload
```

<span data-ttu-id="74ec2-154">Použití klíče pro ssh ověřování (na rozdíl od hesel) se ještě nepodporuje (protože platformou používá ssh pro kopírování balíčků), takže místo toho používat ověřování hesla.</span><span class="sxs-lookup"><span data-stu-id="74ec2-154">Using keys for ssh authentication (as opposed to passwords) isn't yet supported (since the platform uses ssh to copy packages), so use password authentication instead.</span></span>

## <a name="next-steps"></a><span data-ttu-id="74ec2-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74ec2-155">Next steps</span></span>

[<span data-ttu-id="74ec2-156">Nastavení prostředí pro vývoj a nasazení aplikace Service Fabric do clusteru s podporou systému Linux.</span><span class="sxs-lookup"><span data-stu-id="74ec2-156">Set up the development environment and deploy a Service Fabric application to a Linux cluster.</span></span>](service-fabric-get-started-linux.md)

## <a name="related-articles"></a><span data-ttu-id="74ec2-157">Související články</span><span class="sxs-lookup"><span data-stu-id="74ec2-157">Related articles</span></span>

* [<span data-ttu-id="74ec2-158">Začínáme s platformou Service Fabric a Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="74ec2-158">Getting started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
