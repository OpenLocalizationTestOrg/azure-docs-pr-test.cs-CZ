---
title: "nastavení aaaCustom pro prostředí App Service"
description: "Vlastní konfigurace nastavení pro prostředí App Service"
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 1d1d85f3-6cc6-4d57-ae1a-5b37c642d812
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: stefsch
ms.openlocfilehash: 3d140688c88b389e71bfdd465c418339cccab3a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="custom-configuration-settings-for-app-service-environments"></a><span data-ttu-id="26213-103">Vlastní konfigurace nastavení pro prostředí App Service</span><span class="sxs-lookup"><span data-stu-id="26213-103">Custom configuration settings for App Service Environments</span></span>
## <a name="overview"></a><span data-ttu-id="26213-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="26213-104">Overview</span></span>
<span data-ttu-id="26213-105">Protože prostředí App Service jsou izolované tooa jednoho zákazníka, existují určité nastavení konfigurace, které mohou být použity výhradně tooApp služby prostředí.</span><span class="sxs-lookup"><span data-stu-id="26213-105">Because App Service Environments are isolated tooa single customer, there are certain configuration settings that can be applied exclusively tooApp Service Environments.</span></span> <span data-ttu-id="26213-106">Tento článek dokumenty hello různé zvláštními úpravami, které jsou k dispozici pro prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="26213-106">This article documents hello various specific customizations that are available for App Service Environments.</span></span>

<span data-ttu-id="26213-107">Pokud nemáte služby App Service Environment, přečtěte si téma [jak tooCreate služby App Service Environment](app-service-web-how-to-create-an-app-service-environment.md).</span><span class="sxs-lookup"><span data-stu-id="26213-107">If you do not have an App Service Environment, see [How tooCreate an App Service Environment](app-service-web-how-to-create-an-app-service-environment.md).</span></span>

<span data-ttu-id="26213-108">Přizpůsobení App Service Environment může ukládat pomocí pole v hello nové **clusterSettings** atribut.</span><span class="sxs-lookup"><span data-stu-id="26213-108">You can store App Service Environment customizations by using an array in hello new **clusterSettings** attribute.</span></span> <span data-ttu-id="26213-109">Tento atribut je součástí slovník "Vlastnosti" hello hello *hostingEnvironments* entity Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="26213-109">This attribute is found in hello "Properties" dictionary of hello *hostingEnvironments* Azure Resource Manager entity.</span></span>

<span data-ttu-id="26213-110">Hello následující fragment kódu šablony Resource Manageru zkrácené ukazuje hello **clusterSettings** atribut:</span><span class="sxs-lookup"><span data-stu-id="26213-110">hello following abbreviated Resource Manager template snippet shows hello **clusterSettings** attribute:</span></span>

    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

<span data-ttu-id="26213-111">Hello **clusterSettings** atribut můžou být součástí hello tooupdate šablony správce prostředků služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="26213-111">hello **clusterSettings** attribute can be included in a Resource Manager template tooupdate hello App Service Environment.</span></span>

## <a name="use-azure-resource-explorer-tooupdate-an-app-service-environment"></a><span data-ttu-id="26213-112">Použití Průzkumníka prostředků Azure tooupdate služby App Service Environment</span><span class="sxs-lookup"><span data-stu-id="26213-112">Use Azure Resource Explorer tooupdate an App Service Environment</span></span>
<span data-ttu-id="26213-113">Alternativně můžete aktualizovat hello App Service Environment pomocí [Průzkumníka prostředků Azure](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="26213-113">Alternatively, you can update hello App Service Environment by using [Azure Resource Explorer](https://resources.azure.com).</span></span>  

1. <span data-ttu-id="26213-114">V Průzkumníku prostředků, přejděte toohello uzel pro hello App Service Environment (**odběry** > **Skupinyprostředků** > **zprostředkovatelé**  >  **Microsoft.Web** > **hostingEnvironments**).</span><span class="sxs-lookup"><span data-stu-id="26213-114">In Resource Explorer, go toohello node for hello App Service Environment (**subscriptions** > **resourceGroups** > **providers** > **Microsoft.Web** > **hostingEnvironments**).</span></span> <span data-ttu-id="26213-115">Pak klikněte na tlačítko hello konkrétní App Service Environment, které chcete tooupdate.</span><span class="sxs-lookup"><span data-stu-id="26213-115">Then click hello specific App Service Environment that you want tooupdate.</span></span>
2. <span data-ttu-id="26213-116">V pravém podokně hello, klikněte na **pro čtení a zápis** v tooallow horním panelu nástrojů hello interaktivní úpravy v Průzkumníku prostředků.</span><span class="sxs-lookup"><span data-stu-id="26213-116">In hello right pane, click **Read/Write** in hello upper toolbar tooallow interactive editing in Resource Explorer.</span></span>  
3. <span data-ttu-id="26213-117">Klikněte na tlačítko hello blue **upravit** šablony Resource Manageru hello tlačítko toomake je upravovat.</span><span class="sxs-lookup"><span data-stu-id="26213-117">Click hello blue **Edit** button toomake hello Resource Manager template editable.</span></span>
4. <span data-ttu-id="26213-118">Posuňte se konec toohello v pravém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="26213-118">Scroll toohello bottom of hello right pane.</span></span> <span data-ttu-id="26213-119">Hello **clusterSettings** na velmi dolní hello, kde můžete zadat nebo aktualizovat její hodnota je atribut.</span><span class="sxs-lookup"><span data-stu-id="26213-119">hello **clusterSettings** attribute is at hello very bottom, where you can enter or update its value.</span></span>
5. <span data-ttu-id="26213-120">Typ (nebo kopírování a vkládání) hello pole hodnot konfigurace, které chcete v hello **clusterSettings** atribut.</span><span class="sxs-lookup"><span data-stu-id="26213-120">Type (or copy and paste) hello array of configuration values you want in hello **clusterSettings** attribute.</span></span>  
6. <span data-ttu-id="26213-121">Klikněte na zelenou hello **PUT** tlačítko, který je umístěný na začátku hello hello pravém podokně toocommit hello změnu toohello App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="26213-121">Click hello green **PUT** button that's located at hello top of hello right pane toocommit hello change toohello App Service Environment.</span></span>

<span data-ttu-id="26213-122">Ale odešlete změny hello trvá přibližně 30 minut násobí hodnotou hello počet front zakončení v hello App Service Environment pro hello změnit tootake efekt.</span><span class="sxs-lookup"><span data-stu-id="26213-122">However you submit hello change, it takes roughly 30 minutes multiplied by hello number of front ends in hello App Service Environment for hello change tootake effect.</span></span>
<span data-ttu-id="26213-123">Například pokud služby App Service Environment má čtyři front-end, bude trvat zhruba dvě hodiny pro toofinish aktualizace konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="26213-123">For example, if an App Service Environment has four front ends, it will take roughly two hours for hello configuration update toofinish.</span></span> <span data-ttu-id="26213-124">Při změně konfigurace hello je se nasazuje, žádná jiná operace škálování nebo operací změny konfigurace můžete provést v hello App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="26213-124">While hello configuration change is being rolled out, no other scaling operations or configuration change operations can take place in hello App Service Environment.</span></span>

## <a name="disable-tls-10"></a><span data-ttu-id="26213-125">Zakázat protokol TLS 1.0</span><span class="sxs-lookup"><span data-stu-id="26213-125">Disable TLS 1.0</span></span>
<span data-ttu-id="26213-126">Opakované otázku od zákazníků, hlavně zákazníkům, kteří se zabývají soulad s normami PCI audity, je, jak tooexplicitly zákaz protokolu TLS 1.0 pro svoje aplikace.</span><span class="sxs-lookup"><span data-stu-id="26213-126">A recurring question from customers, especially customers who are dealing with PCI compliance audits, is how tooexplicitly disable TLS 1.0 for their apps.</span></span>

<span data-ttu-id="26213-127">Protokol TLS 1.0 je možné zakázat prostřednictvím hello následující **clusterSettings** položku:</span><span class="sxs-lookup"><span data-stu-id="26213-127">TLS 1.0 can be disabled through hello following **clusterSettings** entry:</span></span>

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a><span data-ttu-id="26213-128">Pořadí sady šifer TLS změn</span><span class="sxs-lookup"><span data-stu-id="26213-128">Change TLS cipher suite order</span></span>
<span data-ttu-id="26213-129">Další otázka od zákazníků se pokud mohou upravit hello seznam šifry vyjednaném jejich serveru a toho lze dosáhnout úpravou hello **clusterSettings** jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="26213-129">Another question from customers is if they can modify hello list of ciphers negotiated by their server and this can be achieved by modifying hello **clusterSettings** as shown below.</span></span> <span data-ttu-id="26213-130">seznam Hello k dispozici šifrovací sady lze načíst z [tohoto článku na webu MSDN](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).</span><span class="sxs-lookup"><span data-stu-id="26213-130">hello list of cipher suites available can be retrieved from [this MSDN article](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).</span></span>

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> <span data-ttu-id="26213-131">Pokud nesprávné hodnoty jsou nastavené pro hello šifrovací sada, která SChannel nelze pochopit, všechny TLS komunikace tooyour server může přestat fungovat.</span><span class="sxs-lookup"><span data-stu-id="26213-131">If incorrect values are set for hello cipher suite that SChannel cannot understand, all TLS communication tooyour server might stop functioning.</span></span> <span data-ttu-id="26213-132">V takovém případě budete potřebovat tooremove hello *FrontEndSSLCipherSuiteOrder* položku z **clusterSettings** a odeslání hello aktualizovat Resource Manager šablony toorevert back toohello výchozí šifrovací Sada nastavení.</span><span class="sxs-lookup"><span data-stu-id="26213-132">In such a case, you will need tooremove hello *FrontEndSSLCipherSuiteOrder* entry from **clusterSettings** and submit hello updated Resource Manager template toorevert back toohello default cipher suite settings.</span></span>  <span data-ttu-id="26213-133">Použijte prosím tato funkce se zvýšenou opatrností.</span><span class="sxs-lookup"><span data-stu-id="26213-133">Please use this functionality with caution.</span></span>
> 
> 

## <a name="get-started"></a><span data-ttu-id="26213-134">Začínáme</span><span class="sxs-lookup"><span data-stu-id="26213-134">Get started</span></span>
<span data-ttu-id="26213-135">Web šablony správce prostředků Azure rychlý start Hello obsahuje šablonu s hello základní definice [vytváření služby App Service Environment](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).</span><span class="sxs-lookup"><span data-stu-id="26213-135">hello Azure Quickstart Resource Manager template site includes a template with hello base definition for [creating an App Service Environment](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).</span></span>

<!-- LINKS -->

<!-- IMAGES -->
