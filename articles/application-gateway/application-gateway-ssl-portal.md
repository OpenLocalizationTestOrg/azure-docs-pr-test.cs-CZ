---
title: "aaaConfigure SSL snižování zátěže - Azure Application Gateway - Azure Portal | Microsoft Docs"
description: "Tato stránka obsahuje pokyny toocreate služby application gateway pomocí protokolu SSL snižování zátěže přes portál hello"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 8373379a-a26a-45d2-aa62-dd282298eff3
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: e87ac0bbe10ac45e307c18802741c7bc31764a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-hello-portal"></a><span data-ttu-id="4b40d-103">Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="4b40d-103">Configure an application gateway for SSL offload by using hello portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4b40d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4b40d-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="4b40d-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b40d-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="4b40d-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b40d-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="4b40d-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4b40d-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="4b40d-108">Služba Azure Application Gateway může být relace Secure Sockets Layer (SSL) nakonfigurované tooterminate hello v hello brány tooavoid nákladná SSL dešifrování úlohy toohappen v hello webové farmy.</span><span class="sxs-lookup"><span data-stu-id="4b40d-108">Azure Application Gateway can be configured tooterminate hello Secure Sockets Layer (SSL) session at hello gateway tooavoid costly SSL decryption tasks toohappen at hello web farm.</span></span> <span data-ttu-id="4b40d-109">Přesměrování zpracování SSL zjednodušuje i nastavení serveru front-end hello a Správa webové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4b40d-109">SSL offload also simplifies hello front-end server setup and management of hello web application.</span></span>

## <a name="scenario"></a><span data-ttu-id="4b40d-110">Scénář</span><span class="sxs-lookup"><span data-stu-id="4b40d-110">Scenario</span></span>

<span data-ttu-id="4b40d-111">Následující scénář Hello projde konfigurace přesměrování zpracování SSL na existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4b40d-111">hello following scenario goes through configuring SSL offload on an existing application gateway.</span></span> <span data-ttu-id="4b40d-112">Hello scénář předpokládá, že jste již provedli kroky hello příliš[vytvoření služby Application Gateway](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4b40d-112">hello scenario assumes that you have already followed hello steps too[Create an Application Gateway](application-gateway-create-gateway-portal.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4b40d-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4b40d-113">Before you begin</span></span>

<span data-ttu-id="4b40d-114">tooconfigure přesměrování zpracování SSL pomocí služby application gateway, je vyžadován certifikát.</span><span class="sxs-lookup"><span data-stu-id="4b40d-114">tooconfigure SSL offload with an application gateway, a certificate is required.</span></span> <span data-ttu-id="4b40d-115">Tento certifikát je načteno ve hello application gateway a používá tooencrypt a dešifrování přenosů hello odeslána prostřednictvím protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="4b40d-115">This certificate is loaded on hello application gateway and used tooencrypt and decrypt hello traffic sent via SSL.</span></span> <span data-ttu-id="4b40d-116">certifikát Hello musí toobe formát Personal Information Exchange (pfx).</span><span class="sxs-lookup"><span data-stu-id="4b40d-116">hello certificate needs toobe in Personal Information Exchange (pfx) format.</span></span> <span data-ttu-id="4b40d-117">Tento formát souboru umožňuje pro hello privátní klíče toobe exportovali, které je požadované hello aplikace brány tooperform hello šifrování a dešifrování přenosů.</span><span class="sxs-lookup"><span data-stu-id="4b40d-117">This file format allows for hello private key toobe exported which is required by hello application gateway tooperform hello encryption and decryption of traffic.</span></span>

## <a name="add-an-https-listener"></a><span data-ttu-id="4b40d-118">Přidejte naslouchací proces HTTPS</span><span class="sxs-lookup"><span data-stu-id="4b40d-118">Add an HTTPS listener</span></span>

<span data-ttu-id="4b40d-119">naslouchací proces HTTPS Hello hledá provozu na základě jeho konfigurace a pomáhá trasy hello provoz toohello back-endové fondy.</span><span class="sxs-lookup"><span data-stu-id="4b40d-119">hello HTTPS listener looks for traffic based on its configuration and helps route hello traffic toohello backend pools.</span></span>

### <a name="step-1"></a><span data-ttu-id="4b40d-120">Krok 1</span><span class="sxs-lookup"><span data-stu-id="4b40d-120">Step 1</span></span>

<span data-ttu-id="4b40d-121">Přejděte toohello portál Azure a vyberte existující aplikační brány</span><span class="sxs-lookup"><span data-stu-id="4b40d-121">Navigate toohello Azure portal and select an existing application gateway</span></span>

### <a name="step-2"></a><span data-ttu-id="4b40d-122">Krok 2</span><span class="sxs-lookup"><span data-stu-id="4b40d-122">Step 2</span></span>

<span data-ttu-id="4b40d-123">Klikněte na tlačítko naslouchací procesy a klikněte na tlačítko hello přidat tlačítko tooadd naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="4b40d-123">Click Listeners and click hello Add button tooadd a listener.</span></span>

![okno Přehled brány aplikace][1]

### <a name="step-3"></a><span data-ttu-id="4b40d-125">Krok 3</span><span class="sxs-lookup"><span data-stu-id="4b40d-125">Step 3</span></span>

<span data-ttu-id="4b40d-126">Vyplňte hello požadované informace pro naslouchací proces hello a nahrávání hello certifikátů .pfx, po dokončení klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="4b40d-126">Fill out hello required information for hello listener and upload hello .pfx certificate, when complete click OK.</span></span>

<span data-ttu-id="4b40d-127">**Název** – tato hodnota je popisný název hello naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="4b40d-127">**Name** - This value is a friendly name of hello listener.</span></span>

<span data-ttu-id="4b40d-128">**Konfigurace IP front-endu** – tato hodnota je konfigurace IP front-endu hello, který se používá pro naslouchací proces hello.</span><span class="sxs-lookup"><span data-stu-id="4b40d-128">**Frontend IP configuration** - This value is hello frontend IP configuration that is used for hello listener.</span></span>

<span data-ttu-id="4b40d-129">**Front-endový port (název/Port)** -popisný název hello port používaný na front-endu hello aplikační brány a hello skutečný port používaný hello.</span><span class="sxs-lookup"><span data-stu-id="4b40d-129">**Frontend port (Name/Port)** - A friendly name for hello port used on hello front end of hello application gateway and hello actual port used.</span></span>

<span data-ttu-id="4b40d-130">**Protokol** -toodetermine přepínače, pokud se používá protokol https nebo http pro hello front-endu.</span><span class="sxs-lookup"><span data-stu-id="4b40d-130">**Protocol** - A switch toodetermine if https or http is used for hello front end.</span></span>

<span data-ttu-id="4b40d-131">**Certifikát (jméno a heslo)** – přesměrování zpracování SSL Pokud se používá, je vyžadován pro toto nastavení certifikát .pfx a popisný název a heslo jsou povinné.</span><span class="sxs-lookup"><span data-stu-id="4b40d-131">**Certificate (Name/Password)** - If SSL offload is used, a .pfx certificate is required for this setting and a friendly name and password are required.</span></span>

![Přidejte naslouchací proces okno][2]

## <a name="create-a-rule-and-associate-it-toohello-listener"></a><span data-ttu-id="4b40d-133">Vytvoření pravidla a přidružit ho toohello naslouchací proces</span><span class="sxs-lookup"><span data-stu-id="4b40d-133">Create a rule and associate it toohello listener</span></span>

<span data-ttu-id="4b40d-134">byla vytvořena Hello naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="4b40d-134">hello listener has now been created.</span></span> <span data-ttu-id="4b40d-135">Je čas toocreate přenosem hello toohandle pravidlo z hello naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="4b40d-135">It is time toocreate a rule toohandle hello traffic from hello listener.</span></span> <span data-ttu-id="4b40d-136">Pravidla určují, jak provoz se směruje toohello back-endové fondy na základě více nastavení konfigurace, včetně toho, jestli se používá spřažení na základě souboru cookie relace, protokol, port a sondy stavu.</span><span class="sxs-lookup"><span data-stu-id="4b40d-136">Rules define how traffic is routed toohello backend pools based on multiple configuration settings, including whether cookie-based session affinity is used, protocol, port, and health probes.</span></span>

### <a name="step-1"></a><span data-ttu-id="4b40d-137">Krok 1</span><span class="sxs-lookup"><span data-stu-id="4b40d-137">Step 1</span></span>

<span data-ttu-id="4b40d-138">Klikněte na tlačítko hello **pravidla** hello aplikace brány a pak klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="4b40d-138">Click hello **Rules** of hello application gateway, and then click Add.</span></span>

![okně pravidla aplikace brány][3]

### <a name="step-2"></a><span data-ttu-id="4b40d-140">Krok 2</span><span class="sxs-lookup"><span data-stu-id="4b40d-140">Step 2</span></span>

<span data-ttu-id="4b40d-141">Na hello **přidat základní pravidlo** okno, zadejte popisný název pravidla hello hello a zvolte naslouchací proces hello vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="4b40d-141">On hello **Add basic rule** blade, type in hello friendly name for hello rule and choose hello listener created in hello previous step.</span></span> <span data-ttu-id="4b40d-142">Zvolte hello odpovídající back-endový fond a http nastavení a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="4b40d-142">Choose hello appropriate backend pool and http setting and click **OK**</span></span>

![okno nastavení protokolu HTTPS][4]

<span data-ttu-id="4b40d-144">nastavení Hello se nyní ukládají toohello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="4b40d-144">hello settings are now saved toohello application gateway.</span></span> <span data-ttu-id="4b40d-145">Hello uložit proces pro toto nastavení může chvíli trvat, než jsou k dispozici tooview prostřednictvím hello portálu nebo pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4b40d-145">hello save process for these settings may take a while before they are available tooview through hello portal or through PowerShell.</span></span> <span data-ttu-id="4b40d-146">Aplikační brána jednou uložené hello zpracovává hello šifrování a dešifrování přenosů.</span><span class="sxs-lookup"><span data-stu-id="4b40d-146">Once saved hello application gateway handles hello encryption and decryption of traffic.</span></span> <span data-ttu-id="4b40d-147">Prostřednictvím protokolu http se budou zpracovávat všechny přenosy mezi hello aplikační bránu a webovými servery služby hello back-end.</span><span class="sxs-lookup"><span data-stu-id="4b40d-147">All traffic between hello application gateway and hello backend web servers will be handled over http.</span></span> <span data-ttu-id="4b40d-148">Jakýkoli klient back toohello komunikace Pokud iniciované přes protokol https, bude vrácen toohello klienta zašifrovaná.</span><span class="sxs-lookup"><span data-stu-id="4b40d-148">Any communication back toohello client if initiated over https will be returned toohello client encrypted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b40d-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b40d-149">Next steps</span></span>

<span data-ttu-id="4b40d-150">toolearn způsobu tooconfigure vlastní stavu testu s Azure Application Gateway, najdete v [vytvořit sondu vlastní stavu](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4b40d-150">toolearn how tooconfigure a custom health probe with Azure Application Gateway, see [Create a custom health probe](application-gateway-create-gateway-portal.md).</span></span>

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png
