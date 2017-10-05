---
title: "Přidat certifikát do úložiště certifikační Autority Java | Microsoft Docs"
description: "Zjistěte, jak přidat certifikát certifikační autority (CA) do úložiště certifikátů (cacerts) Java CA pro Twilio služby nebo Azure Service Bus."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: d3699b0a-835c-43fb-844d-9c25344e5cda
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 4f3ec837588c6e959e82108ca25ab4289e40d3f5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a><span data-ttu-id="da38d-103">Přidání certifikátu do úložiště certifikátů certifikační Autority Java</span><span class="sxs-lookup"><span data-stu-id="da38d-103">Adding a Certificate to the Java CA Certificates Store</span></span>
<span data-ttu-id="da38d-104">Následující kroky ukazují, jak přidat certifikát certifikační autority (CA) do úložiště certifikátů (cacerts) Java certifikační Autority.</span><span class="sxs-lookup"><span data-stu-id="da38d-104">The following steps show you how to add a certificate authority (CA) certificate to the Java CA certificate (cacerts) store.</span></span> <span data-ttu-id="da38d-105">Příklad používá se pro certifikát certifikační Autority vyžadované službou Twilio.</span><span class="sxs-lookup"><span data-stu-id="da38d-105">The example used is for the CA certificate required by the Twilio service.</span></span> <span data-ttu-id="da38d-106">Informace uvedené dále v tomto tématu popisuje postup instalace certifikátu certifikační Autority pro Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="da38d-106">Information provided later in the topic describes how to install the CA certificate for the Azure Service Bus.</span></span> 

<span data-ttu-id="da38d-107">Přidání certifikátu certifikační Autority před pomocí formátu ZIP vaší JDK a její přidání do projektu Azure můžete použít keytool **approot** složky, nebo může spustit úkol aplikace Azure spuštění, který používá keytool se přidat certifikát.</span><span class="sxs-lookup"><span data-stu-id="da38d-107">You can use keytool to add the CA certificate prior to zipping your JDK and adding it to your Azure project's **approot** folder, or you could run an Azure start-up task that uses keytool to add the certificate.</span></span> <span data-ttu-id="da38d-108">Tento příklad předpokládá, že přidáte certifikát Certifikační autority před JDK se metoda ZIP.</span><span class="sxs-lookup"><span data-stu-id="da38d-108">This example assumes you will add a CA certificate prior to the JDK being zipped.</span></span> <span data-ttu-id="da38d-109">Také v příkladu se použije konkrétní certifikát certifikační Autority, ale postup získání jiný certifikát certifikační Autority a jeho import do úložiště cacerts by být podobné.</span><span class="sxs-lookup"><span data-stu-id="da38d-109">Also, a specific CA certificate will be used in the example, but the steps of obtaining a different CA certificate and importing it into the cacerts store would be similar.</span></span>

## <a name="to-add-a-certificate-to-the-cacerts-store"></a><span data-ttu-id="da38d-110">Přidat certifikát do úložiště cacerts</span><span class="sxs-lookup"><span data-stu-id="da38d-110">To add a certificate to the cacerts store</span></span>
1. <span data-ttu-id="da38d-111">Na příkazovém řádku, který je nastaven na vaše JDK **jdk\jre\lib\security** složky, spusťte následující příkaz k instalaci jaké certifikátů:</span><span class="sxs-lookup"><span data-stu-id="da38d-111">At a command prompt that is set to your JDK's **jdk\jre\lib\security** folder, run the following to see what certificates are installed:</span></span>
   
    `keytool -list -keystore cacerts`
   
    <span data-ttu-id="da38d-112">Budete vyzváni k zadání hesla, úložiště.</span><span class="sxs-lookup"><span data-stu-id="da38d-112">You'll be prompted for the store password.</span></span> <span data-ttu-id="da38d-113">Výchozí heslo je **changeit**.</span><span class="sxs-lookup"><span data-stu-id="da38d-113">The default password is **changeit**.</span></span> <span data-ttu-id="da38d-114">(Pokud chcete změnit heslo, naleznete v dokumentaci příkazu keytool na <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) Tento příklad předpokládá, že certifikát s MD5 otisků 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 není uvedena v seznamu a, kterou chcete importovat (Tato konkrétní certifikát je vyžadována služba Twilio rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="da38d-114">(If you want to change the password, see the keytool documentation at <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) This example assumes that the certificate with MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 is not listed, and that you want to import it (this particular certificate is needed by the Twilio API service).</span></span>
2. <span data-ttu-id="da38d-115">Tento certifikát získat ze seznamu certifikátů, které jsou uvedeny v [GeoTrust kořenové certifikáty](http://www.geotrust.com/resources/root-certificates/).</span><span class="sxs-lookup"><span data-stu-id="da38d-115">Obtain the certificate from the list of certificates listed at [GeoTrust Root Certificates](http://www.geotrust.com/resources/root-certificates/).</span></span> <span data-ttu-id="da38d-116">Klikněte pravým tlačítkem myši na odkaz pro certifikát s 35:DE:F4:CF sériové číslo a uložte ho do **jdk\jre\lib\security** složky.</span><span class="sxs-lookup"><span data-stu-id="da38d-116">Right-click the link for the certificate with serial number 35:DE:F4:CF and save it to the **jdk\jre\lib\security** folder.</span></span> <span data-ttu-id="da38d-117">Pro účely tohoto příkladu se uloží do souboru s názvem **Equifax\_zabezpečeného\_certifikát\_Authority.cer**.</span><span class="sxs-lookup"><span data-stu-id="da38d-117">For purposes of this example, it was saved to a file named **Equifax\_Secure\_Certificate\_Authority.cer**.</span></span>
3. <span data-ttu-id="da38d-118">Importujte certifikát pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="da38d-118">Import the certificate via the following command:</span></span>
   
    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`
   
    <span data-ttu-id="da38d-119">Po zobrazení výzvy k tomuto certifikátu důvěřovat, pokud má certifikát MD5 otisk prstu 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 reakce zadáním **y**.</span><span class="sxs-lookup"><span data-stu-id="da38d-119">When prompted to trust this certificate, if the certificate has MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, respond by typing **y**.</span></span>
4. <span data-ttu-id="da38d-120">Spusťte následující příkaz, který zajistěte, aby byl že certifikát certifikační Autority musí být úspěšně naimportována:</span><span class="sxs-lookup"><span data-stu-id="da38d-120">Run the following command to ensure the CA certificate has been successfully imported:</span></span>
   
    `keytool -list -keystore cacerts`
5. <span data-ttu-id="da38d-121">Sadu JDK ZIP a přidejte ji do projektu Azure **approot** složky.</span><span class="sxs-lookup"><span data-stu-id="da38d-121">Zip the JDK and add it to your Azure project's **approot** folder.</span></span>

<span data-ttu-id="da38d-122">Informace o příkazu keytool najdete v tématu <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span><span class="sxs-lookup"><span data-stu-id="da38d-122">For information about keytool, see <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span></span>

## <a name="azure-root-certificates"></a><span data-ttu-id="da38d-123">Azure kořenové certifikáty</span><span class="sxs-lookup"><span data-stu-id="da38d-123">Azure Root Certificates</span></span>
<span data-ttu-id="da38d-124">Aplikace, které používají služby Azure (například Azure Service Bus) musí důvěřovat certifikátu Baltimore CyberTrust Root.</span><span class="sxs-lookup"><span data-stu-id="da38d-124">Your applications that use Azure services (such as Azure Service Bus) need to trust the Baltimore CyberTrust Root certificate.</span></span> <span data-ttu-id="da38d-125">(Od 15. dubna 2013, Azure začal migrace z GTE CyberTrust Root globální na Baltimore CyberTrust Root.</span><span class="sxs-lookup"><span data-stu-id="da38d-125">(Beginning April 15, 2013, Azure began migrating from the GTE CyberTrust Global Root to the Baltimore CyberTrust Root.</span></span> <span data-ttu-id="da38d-126">Tato migrace trvalo několik měsíců).</span><span class="sxs-lookup"><span data-stu-id="da38d-126">This migration took several months to complete.)</span></span>

<span data-ttu-id="da38d-127">Baltimore certifikát již je nainstalován v úložišti cacerts, takže nezapomeňte spustit **keytool-seznamu** příkaz nejprve se, zda již existuje.</span><span class="sxs-lookup"><span data-stu-id="da38d-127">The Baltimore certificate might already be installed in your cacerts store, so remember to run the **keytool -list** command first to see if it already exists.</span></span>

<span data-ttu-id="da38d-128">Pokud potřebujete přidat Baltimore CyberTrust Root, má 02:00:00:b9 sériové číslo a SHA1 otisk prstu d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74.</span><span class="sxs-lookup"><span data-stu-id="da38d-128">If you need to add the Baltimore CyberTrust Root, it has serial number 02:00:00:b9 and SHA1 fingerprint d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74.</span></span> <span data-ttu-id="da38d-129">Lze ji stáhnout z <https://cacert.omniroot.com/bc2025.crt>, uloží se do místního souboru s příponou **.cer**a poté importovat pomocí **keytool** jako v příkladu nahoře.</span><span class="sxs-lookup"><span data-stu-id="da38d-129">It can be downloaded from <https://cacert.omniroot.com/bc2025.crt>, saved to a local file with extension **.cer**, and then imported using **keytool** as shown above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="da38d-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="da38d-130">Next steps</span></span>
<span data-ttu-id="da38d-131">Další informace o kořenové certifikáty používají v Azure najdete v tématu [Azure kořenový certifikát migrace](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).</span><span class="sxs-lookup"><span data-stu-id="da38d-131">For more information about the root certificates used by Azure, see [Azure Root Certificate Migration](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).</span></span>

<span data-ttu-id="da38d-132">Další informace o Java najdete v tématu [Azure pro vývojáře v jazyce Java](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="da38d-132">For more information about Java, see [Azure for Java developers](/java/azure).</span></span>

