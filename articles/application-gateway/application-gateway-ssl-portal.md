---
title: "Konfigurovat přesměrování zpracování SSL - Azure Application Gateway - portálu Azure | Microsoft Docs"
description: "Tato stránka poskytuje pokyny pro vytvoření služby application gateway pomocí protokolu SSL, přesměrování zpracování úloh pomocí portálu"
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
ms.openlocfilehash: f61be0cc4c9274c9914f7c468ce48a2a3d0a4f4a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a><span data-ttu-id="b53fa-103">Konfigurace aplikační brány pro přesměrování zpracování SSL pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="b53fa-103">Configure an application gateway for SSL offload by using the portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b53fa-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b53fa-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="b53fa-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="b53fa-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="b53fa-106">Azure Classic PowerShell</span><span class="sxs-lookup"><span data-stu-id="b53fa-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="b53fa-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b53fa-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="b53fa-108">Služba Azure Application Gateway se dá nakonfigurovat k ukončení relace Secure Sockets Layer (SSL) v bráně, vyhnete se tak nákladným úlohám dešifrování SSL na webové serverové farmě.</span><span class="sxs-lookup"><span data-stu-id="b53fa-108">Azure Application Gateway can be configured to terminate the Secure Sockets Layer (SSL) session at the gateway to avoid costly SSL decryption tasks to happen at the web farm.</span></span> <span data-ttu-id="b53fa-109">Přesměrování zpracování SSL zjednodušuje i nastavení a správu front-end serverů webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="b53fa-109">SSL offload also simplifies the front-end server setup and management of the web application.</span></span>

## <a name="scenario"></a><span data-ttu-id="b53fa-110">Scénář</span><span class="sxs-lookup"><span data-stu-id="b53fa-110">Scenario</span></span>

<span data-ttu-id="b53fa-111">V následujícím scénáři projde konfigurace přesměrování zpracování SSL na existující aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="b53fa-111">The following scenario goes through configuring SSL offload on an existing application gateway.</span></span> <span data-ttu-id="b53fa-112">Tento scénář předpokládá, že jste již provedli postup [vytvoření služby Application Gateway](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b53fa-112">The scenario assumes that you have already followed the steps to [Create an Application Gateway](application-gateway-create-gateway-portal.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b53fa-113">Než začnete</span><span class="sxs-lookup"><span data-stu-id="b53fa-113">Before you begin</span></span>

<span data-ttu-id="b53fa-114">Pokud chcete konfigurovat přesměrování zpracování SSL pomocí služby application gateway, je vyžadován certifikát.</span><span class="sxs-lookup"><span data-stu-id="b53fa-114">To configure SSL offload with an application gateway, a certificate is required.</span></span> <span data-ttu-id="b53fa-115">Tento certifikát je načíst ve službě application gateway a používat k šifrování a dešifrování přenosy odesílané prostřednictvím protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="b53fa-115">This certificate is loaded on the application gateway and used to encrypt and decrypt the traffic sent via SSL.</span></span> <span data-ttu-id="b53fa-116">Certifikát musí být ve formátu Personal Information Exchange (pfx).</span><span class="sxs-lookup"><span data-stu-id="b53fa-116">The certificate needs to be in Personal Information Exchange (pfx) format.</span></span> <span data-ttu-id="b53fa-117">Tento formát souboru umožňuje pro export privátního klíče, který je vyžadován součástí služby application gateway k šifrování a dešifrování přenosů.</span><span class="sxs-lookup"><span data-stu-id="b53fa-117">This file format allows for the private key to be exported which is required by the application gateway to perform the encryption and decryption of traffic.</span></span>

## <a name="add-an-https-listener"></a><span data-ttu-id="b53fa-118">Přidejte naslouchací proces HTTPS</span><span class="sxs-lookup"><span data-stu-id="b53fa-118">Add an HTTPS listener</span></span>

<span data-ttu-id="b53fa-119">Naslouchací proces HTTPS hledá provozu na základě jeho konfigurace a pomáhá směrovat přenosy back-endové fondy.</span><span class="sxs-lookup"><span data-stu-id="b53fa-119">The HTTPS listener looks for traffic based on its configuration and helps route the traffic to the backend pools.</span></span>

### <a name="step-1"></a><span data-ttu-id="b53fa-120">Krok 1</span><span class="sxs-lookup"><span data-stu-id="b53fa-120">Step 1</span></span>

<span data-ttu-id="b53fa-121">Přejděte na portál Azure a vyberte existující aplikační brány</span><span class="sxs-lookup"><span data-stu-id="b53fa-121">Navigate to the Azure portal and select an existing application gateway</span></span>

### <a name="step-2"></a><span data-ttu-id="b53fa-122">Krok 2</span><span class="sxs-lookup"><span data-stu-id="b53fa-122">Step 2</span></span>

<span data-ttu-id="b53fa-123">Klikněte na tlačítko naslouchací procesy a klikněte na tlačítko Přidat přidejte naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="b53fa-123">Click Listeners and click the Add button to add a listener.</span></span>

![okno Přehled brány aplikace][1]

### <a name="step-3"></a><span data-ttu-id="b53fa-125">Krok 3</span><span class="sxs-lookup"><span data-stu-id="b53fa-125">Step 3</span></span>

<span data-ttu-id="b53fa-126">Zadejte požadované informace pro naslouchací proces a nahrání certifikátu .pfx, po dokončení klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="b53fa-126">Fill out the required information for the listener and upload the .pfx certificate, when complete click OK.</span></span>

<span data-ttu-id="b53fa-127">**Název** – tato hodnota je popisný název naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="b53fa-127">**Name** - This value is a friendly name of the listener.</span></span>

<span data-ttu-id="b53fa-128">**Konfigurace IP front-endu** – tato hodnota je front-endovou konfiguraci protokolu IP, který se používá pro naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="b53fa-128">**Frontend IP configuration** - This value is the frontend IP configuration that is used for the listener.</span></span>

<span data-ttu-id="b53fa-129">**Front-endový port (název/Port)** – popisný název pro tento port používají na front-endu aplikační brány a skutečný port používat.</span><span class="sxs-lookup"><span data-stu-id="b53fa-129">**Frontend port (Name/Port)** - A friendly name for the port used on the front end of the application gateway and the actual port used.</span></span>

<span data-ttu-id="b53fa-130">**Protokol** -přepínače k určení, pokud se používá protokol https nebo http pro front-endu.</span><span class="sxs-lookup"><span data-stu-id="b53fa-130">**Protocol** - A switch to determine if https or http is used for the front end.</span></span>

<span data-ttu-id="b53fa-131">**Certifikát (jméno a heslo)** – přesměrování zpracování SSL Pokud se používá, je vyžadován pro toto nastavení certifikát .pfx a popisný název a heslo jsou povinné.</span><span class="sxs-lookup"><span data-stu-id="b53fa-131">**Certificate (Name/Password)** - If SSL offload is used, a .pfx certificate is required for this setting and a friendly name and password are required.</span></span>

![Přidejte naslouchací proces okno][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a><span data-ttu-id="b53fa-133">Vytvoření pravidla a přidružit ho k naslouchacímu procesu</span><span class="sxs-lookup"><span data-stu-id="b53fa-133">Create a rule and associate it to the listener</span></span>

<span data-ttu-id="b53fa-134">Byla vytvořena naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="b53fa-134">The listener has now been created.</span></span> <span data-ttu-id="b53fa-135">Je čas k vytvoření pravidla pro zpracování provozu z naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="b53fa-135">It is time to create a rule to handle the traffic from the listener.</span></span> <span data-ttu-id="b53fa-136">Pravidla určují, jak se provoz směruje na back-endové fondy na základě více nastavení konfigurace, včetně toho, jestli se používá spřažení na základě souboru cookie relace, protokol, port a sondy stavu.</span><span class="sxs-lookup"><span data-stu-id="b53fa-136">Rules define how traffic is routed to the backend pools based on multiple configuration settings, including whether cookie-based session affinity is used, protocol, port, and health probes.</span></span>

### <a name="step-1"></a><span data-ttu-id="b53fa-137">Krok 1</span><span class="sxs-lookup"><span data-stu-id="b53fa-137">Step 1</span></span>

<span data-ttu-id="b53fa-138">Klikněte **pravidla** aplikační brány a pak klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="b53fa-138">Click the **Rules** of the application gateway, and then click Add.</span></span>

![okně pravidla aplikace brány][3]

### <a name="step-2"></a><span data-ttu-id="b53fa-140">Krok 2</span><span class="sxs-lookup"><span data-stu-id="b53fa-140">Step 2</span></span>

<span data-ttu-id="b53fa-141">Na **přidat základní pravidlo** okno, zadejte popisný název pravidla a zvolte naslouchací proces vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="b53fa-141">On the **Add basic rule** blade, type in the friendly name for the rule and choose the listener created in the previous step.</span></span> <span data-ttu-id="b53fa-142">Zvolte odpovídající back-endový fond a http nastavení a klikněte na tlačítko **OK**</span><span class="sxs-lookup"><span data-stu-id="b53fa-142">Choose the appropriate backend pool and http setting and click **OK**</span></span>

![okno nastavení protokolu HTTPS][4]

<span data-ttu-id="b53fa-144">Nastavení jsou nyní uloženy do služby application gateway.</span><span class="sxs-lookup"><span data-stu-id="b53fa-144">The settings are now saved to the application gateway.</span></span> <span data-ttu-id="b53fa-145">Proces ukládání pro toto nastavení může chvíli trvat, než jsou k dispozici k zobrazení prostřednictvím portálu nebo pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b53fa-145">The save process for these settings may take a while before they are available to view through the portal or through PowerShell.</span></span> <span data-ttu-id="b53fa-146">Po uložení služby application gateway zpracovává šifrování a dešifrování přenosů.</span><span class="sxs-lookup"><span data-stu-id="b53fa-146">Once saved the application gateway handles the encryption and decryption of traffic.</span></span> <span data-ttu-id="b53fa-147">Prostřednictvím protokolu http se budou zpracovávat všechny přenosy mezi aplikační bránu a webovými servery back-end.</span><span class="sxs-lookup"><span data-stu-id="b53fa-147">All traffic between the application gateway and the backend web servers will be handled over http.</span></span> <span data-ttu-id="b53fa-148">Veškeré komunikace zpět do klienta, pokud iniciované přes protokol https, bude vrácen klientovi zašifrovaná.</span><span class="sxs-lookup"><span data-stu-id="b53fa-148">Any communication back to the client if initiated over https will be returned to the client encrypted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b53fa-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b53fa-149">Next steps</span></span>

<span data-ttu-id="b53fa-150">Pokud chcete dozvědět, jak nakonfigurovat vlastní stav testu s Azure Application Gateway, přečtěte si téma [vytvořit sondu vlastní stavu](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b53fa-150">To learn how to configure a custom health probe with Azure Application Gateway, see [Create a custom health probe](application-gateway-create-gateway-portal.md).</span></span>

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png
