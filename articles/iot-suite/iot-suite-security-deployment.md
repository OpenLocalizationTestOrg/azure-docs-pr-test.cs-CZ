---
title: "Zabezpečit vaše nasazení Internet věcí | Microsoft Docs"
description: "Tento článek podrobnosti o tom, jak zabezpečit vaše nasazení IoT"
services: 
suite: iot-suite
documentationcenter: 
author: YuriDio
manager: timlt
editor: 
ms.assetid: 95c23341-16b0-4954-b3f2-d2e82ab7b367
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: yurid
ms.openlocfilehash: d752dd13b138cdae80dac5c0b2f84a19fe0aa670
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="secure-your-iot-deployment"></a><span data-ttu-id="937ab-103">Zabezpečení nasazení IoT</span><span class="sxs-lookup"><span data-stu-id="937ab-103">Secure your IoT deployment</span></span>
<span data-ttu-id="937ab-104">Tento článek poskytuje další úroveň podrobností pro zabezpečení infrastruktury založené na Azure IoT Internet věcí (IoT).</span><span class="sxs-lookup"><span data-stu-id="937ab-104">This article provides the next level of detail for securing the Azure IoT-based Internet of Things (IoT) infrastructure.</span></span> <span data-ttu-id="937ab-105">Odkazuje úrovně podrobnosti implementace pro konfiguraci a nasazení jednotlivých součástí.</span><span class="sxs-lookup"><span data-stu-id="937ab-105">It links to implementation level details for configuring and deploying each component.</span></span> <span data-ttu-id="937ab-106">Poskytuje taky porovnání a možnosti mezi různé konkurenční metody.</span><span class="sxs-lookup"><span data-stu-id="937ab-106">It also provides comparisons and choices between various competing methods.</span></span>

<span data-ttu-id="937ab-107">Zabezpečení Azure IoT nasazení je možné rozdělit do těchto tří zabezpečení oblastí:</span><span class="sxs-lookup"><span data-stu-id="937ab-107">Securing the Azure IoT deployment can be divided into the following three security areas:</span></span>

* <span data-ttu-id="937ab-108">**Zabezpečení zařízení**: při nasazení v zástupné zabezpečení zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="937ab-108">**Device Security**: Securing the IoT device while it is deployed in the wild.</span></span>
* <span data-ttu-id="937ab-109">**Zabezpečení připojení**: zajištění všechna data přenášená mezi zařízení IoT a IoT Hub je důvěrný a manipulací.</span><span class="sxs-lookup"><span data-stu-id="937ab-109">**Connection Security**: Ensuring all data transmitted between the IoT device and IoT Hub is confidential and tamper-proof.</span></span>
* <span data-ttu-id="937ab-110">**Zabezpečení cloudu**: poskytnutím prostředků k zabezpečení dat, zatímco prochází přes a je uložen v cloudu.</span><span class="sxs-lookup"><span data-stu-id="937ab-110">**Cloud Security**: Providing a means to secure data while it moves through, and is stored in the cloud.</span></span>

![Tři oblasti zabezpečení][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a><span data-ttu-id="937ab-112">Zabezpečené zřizování zařízení a ověřování</span><span class="sxs-lookup"><span data-stu-id="937ab-112">Secure device provisioning and authentication</span></span>
<span data-ttu-id="937ab-113">Azure IoT Suite zabezpečuje zařízení IoT pomocí následujících dvou metod:</span><span class="sxs-lookup"><span data-stu-id="937ab-113">The Azure IoT Suite secures IoT devices by the following two methods:</span></span>

* <span data-ttu-id="937ab-114">Tím, že pro každé zařízení, která umožňuje zařízením komunikovat s centrem IoT poskytuje jedinečnou identitu klíč (tokeny zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="937ab-114">By providing a unique identity key (security tokens) for each device, which can be used by the device to communicate with the IoT Hub.</span></span>
* <span data-ttu-id="937ab-115">Pomocí na zařízení [certifikát X.509] [ lnk-x509] a privátní klíč jako prostředek k ověření zařízení do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="937ab-115">By using an on-device [X.509 certificate][lnk-x509] and private key as a means to authenticate the device to the IoT Hub.</span></span> <span data-ttu-id="937ab-116">Tato metoda ověřování zajišťuje, že privátní klíč v zařízení není znám mimo zařízení kdykoli, poskytuje vyšší úroveň zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="937ab-116">This authentication method ensures that the private key on the device is not known outside the device at any time, providing a higher level of security.</span></span>

<span data-ttu-id="937ab-117">V případě metody token zabezpečení poskytuje ověření pro každé volání zařízení do služby IoT Hub tím, že přidružíte symetrický klíč pro každé volání.</span><span class="sxs-lookup"><span data-stu-id="937ab-117">The security token method provides authentication for each call made by the device to IoT Hub by associating the symmetric key to each call.</span></span> <span data-ttu-id="937ab-118">Ověřování na základě X.509 umožňuje ověřování zařízení IoT ve fyzické vrstvě jako součást navázání připojení protokol TLS.</span><span class="sxs-lookup"><span data-stu-id="937ab-118">X.509-based authentication allows authentication of an IoT device at the physical layer as part of the TLS connection establishment.</span></span> <span data-ttu-id="937ab-119">Bez ověřování X.509, což je méně bezpečné vzor lze metodu na základě zabezpečení token.</span><span class="sxs-lookup"><span data-stu-id="937ab-119">The security-token-based method can be used without the X.509 authentication which is a less secure pattern.</span></span> <span data-ttu-id="937ab-120">Volba mezi tyto dvě metody je primárně určen míry budou zabezpečené zařízení ověřování musí být a dostupnost zabezpečeného úložiště v zařízení (bezpečně uložit privátní klíč).</span><span class="sxs-lookup"><span data-stu-id="937ab-120">The choice between the two methods is primarily dictated by how secure the device authentication needs to be, and availability of secure storage on the device (to store the private key securely).</span></span>

## <a name="iot-hub-security-tokens"></a><span data-ttu-id="937ab-121">Tokeny zabezpečení IoT Hub</span><span class="sxs-lookup"><span data-stu-id="937ab-121">IoT Hub security tokens</span></span>
<span data-ttu-id="937ab-122">IoT Hub používá tokeny zabezpečení k ověřování zařízení a služby se odesílání klíčů v síti.</span><span class="sxs-lookup"><span data-stu-id="937ab-122">IoT Hub uses security tokens to authenticate devices and services to avoid sending keys on the network.</span></span> <span data-ttu-id="937ab-123">Kromě toho mají omezenou dobu platnosti a obor tokeny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="937ab-123">Additionally, security tokens are limited in time validity and scope.</span></span> <span data-ttu-id="937ab-124">Sady SDK služby Azure IoT automaticky generovat tokeny bez nutnosti žádnou zvláštní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="937ab-124">Azure IoT SDKs automatically generate tokens without requiring any special configuration.</span></span> <span data-ttu-id="937ab-125">Některé scénáře, ale vyžadují uživatele pro vygenerování a použití tokenů zabezpečení přímo.</span><span class="sxs-lookup"><span data-stu-id="937ab-125">Some scenarios, however, require the user to generate and use security tokens directly.</span></span> <span data-ttu-id="937ab-126">Mezi ně patří přímého použití MQTT, AMQP nebo HTTP ploch nebo implementace vzoru služby tokenů.</span><span class="sxs-lookup"><span data-stu-id="937ab-126">These include the direct use of the MQTT, AMQP, or HTTP surfaces, or the implementation of the token service pattern.</span></span>

<span data-ttu-id="937ab-127">Další informace o struktuře token zabezpečení a jeho použití naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="937ab-127">More details on the structure of the security token and its usage can be found in the following articles:</span></span>

* <span data-ttu-id="937ab-128">[Struktura tokenu zabezpečení][lnk-security-tokens]</span><span class="sxs-lookup"><span data-stu-id="937ab-128">[Security token structure][lnk-security-tokens]</span></span>
* <span data-ttu-id="937ab-129">[Pomocí SAS tokeny jako zařízení][lnk-sas-tokens]</span><span class="sxs-lookup"><span data-stu-id="937ab-129">[Using SAS tokens as a device][lnk-sas-tokens]</span></span>

<span data-ttu-id="937ab-130">Každé centrum IoT má [registru identit] [ lnk-identity-registry] které lze použít k vytvoření prostředků na zařízení v rámci služby, jako je například fronty, který obsahuje neukládají zprávy typu cloud zařízení a k povolení přístupu k zařízení přístupem koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="937ab-130">Each IoT Hub has an [identity registry][lnk-identity-registry] that can be used to create per-device resources in the service, such as a queue that contains in-flight cloud-to-device messages, and to allow access to the device-facing endpoints.</span></span> <span data-ttu-id="937ab-131">Registr identit služby IoT Hub poskytuje zabezpečené úložiště identit zařízení a zabezpečení klíčů pro řešení.</span><span class="sxs-lookup"><span data-stu-id="937ab-131">The IoT Hub identity registry provides secure storage of device identities and security keys for a solution.</span></span> <span data-ttu-id="937ab-132">Jednotlivé nebo skupiny identit zařízení lze přidat na seznam povolených nebo blokovaných, povolení plnou kontrolu nad přístup k zařízení.</span><span class="sxs-lookup"><span data-stu-id="937ab-132">Individual or groups of device identities can be added to an allow list, or a block list, enabling complete control over device access.</span></span> <span data-ttu-id="937ab-133">Následující články poskytují další informace o struktuře registru identit a podporované operace.</span><span class="sxs-lookup"><span data-stu-id="937ab-133">The following articles provide more details on the structure of the identity registry and supported operations.</span></span>

<span data-ttu-id="937ab-134">[IoT Hub podporuje protokoly, například MQTT, AMQP a HTTP][lnk-protocols].</span><span class="sxs-lookup"><span data-stu-id="937ab-134">[IoT Hub supports protocols such as MQTT, AMQP, and HTTP][lnk-protocols].</span></span> <span data-ttu-id="937ab-135">Každý z těchto protokolů jinak použijte tokeny zabezpečení ze zařízení IoT do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="937ab-135">Each of these protocols use security tokens from the IoT device to IoT Hub differently:</span></span>

* <span data-ttu-id="937ab-136">AMQP: SASL prostý a založené na deklaracích AMQP zabezpečení ({policyName}@sas.root. { iothubName} v případě tokeny úrovni centra IoT; {deviceId} v případě zařízení obor tokeny).</span><span class="sxs-lookup"><span data-stu-id="937ab-136">AMQP: SASL PLAIN and AMQP Claims-based security ({policyName}@sas.root.{iothubName} in the case of IoT hub-level tokens; {deviceId} in case of device-scoped tokens).</span></span>
* <span data-ttu-id="937ab-137">MQTT: Připojení paketu používá {deviceId} jako {ClientId}, {IoThubhostname} / {deviceId} v **uživatelské jméno** pole a SAS token v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="937ab-137">MQTT: CONNECT packet uses {deviceId} as the {ClientId}, {IoThubhostname}/{deviceId} in the **Username** field and a SAS token in the **Password** field.</span></span>
* <span data-ttu-id="937ab-138">HTTP: Je platný token v hlavičce autorizace požadavku.</span><span class="sxs-lookup"><span data-stu-id="937ab-138">HTTP: Valid token is in the authorization request header.</span></span>

<span data-ttu-id="937ab-139">Registr identit služby IoT Hub lze použít ke konfiguraci zařízení zabezpečovací přihlašovací údaje a řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="937ab-139">IoT Hub identity registry can be used to configure per-device security credentials and access control.</span></span> <span data-ttu-id="937ab-140">Však řešení IoT již má významné investice v [vlastní zařízení identity registru nebo ověřování schématu][lnk-custom-auth], lze ji integrovat do existující infrastruktury s centrem IoT vytvořením služby tokenů.</span><span class="sxs-lookup"><span data-stu-id="937ab-140">However, if an IoT solution already has a significant investment in a [custom device identity registry and/or authentication scheme][lnk-custom-auth], it can be integrated into an existing infrastructure with IoT Hub by creating a token service.</span></span>

### <a name="x509-certificate-based-device-authentication"></a><span data-ttu-id="937ab-141">Ověřování zařízení na základě certifikátu X.509</span><span class="sxs-lookup"><span data-stu-id="937ab-141">X.509 certificate-based device authentication</span></span>
<span data-ttu-id="937ab-142">Použití [zařízení na základě certifikátu X.509] [ lnk-protocols] a jeho přidružený privátní a veřejné klíče dvojice povoluje další ověřování ve fyzické vrstvě.</span><span class="sxs-lookup"><span data-stu-id="937ab-142">The use of a [device-based X.509 certificate][lnk-protocols] and its associated private and public key pair allows additional authentication at the physical layer.</span></span> <span data-ttu-id="937ab-143">Privátní klíč je bezpečně uloženo na zařízení a není zjistitelný mimo zařízení.</span><span class="sxs-lookup"><span data-stu-id="937ab-143">The private key is stored securely in the device and is not discoverable outside the device.</span></span> <span data-ttu-id="937ab-144">Certifikát X.509 obsahuje informace o zařízení, jako je například ID zařízení a další podrobnosti organizace.</span><span class="sxs-lookup"><span data-stu-id="937ab-144">The X.509 certificate contains information about the device, such as device ID, and other organizational details.</span></span> <span data-ttu-id="937ab-145">Podpis certifikátu je vytvořen pomocí soukromého klíče.</span><span class="sxs-lookup"><span data-stu-id="937ab-145">A signature of the certificate is generated by using the private key.</span></span>

<span data-ttu-id="937ab-146">Zřizování toku vysoké úrovně zařízení:</span><span class="sxs-lookup"><span data-stu-id="937ab-146">High-level device provisioning flow:</span></span>

* <span data-ttu-id="937ab-147">Přidružte identifikátor fyzického zařízení – identitu zařízení a/nebo certifikát X.509 přidružené k zařízení během zařízení výrobní nebo uvedení do provozu.</span><span class="sxs-lookup"><span data-stu-id="937ab-147">Associate an identifier to a physical device – device identity and/or X.509 certificate associated to the device during device manufacturing or commissioning.</span></span>
* <span data-ttu-id="937ab-148">Vytvořte záznam odpovídající identity ve IoT Hub – identitu zařízení a informace o přidružených zařízení v registru identit služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="937ab-148">Create a corresponding identity entry in IoT Hub – device identity and associated device information in the IoT Hub identity registry.</span></span>
* <span data-ttu-id="937ab-149">Kryptografický otisk certifikátu X.509 bezpečně uložte v registru identit služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="937ab-149">Securely store X.509 certificate thumbprint in IoT Hub identity registry.</span></span>

### <a name="root-certificate-on-device"></a><span data-ttu-id="937ab-150">Kořenový certifikát na zařízení</span><span class="sxs-lookup"><span data-stu-id="937ab-150">Root certificate on device</span></span>
<span data-ttu-id="937ab-151">Při navazování připojení TLS zabezpečené službou IoT Hub, ověřuje zařízení IoT pomocí kořenový certifikát, který je součástí sady SDK zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="937ab-151">While establishing a secure TLS connection with IoT Hub, the IoT device authenticates IoT Hub using a root certificate which is part of the device SDK.</span></span> <span data-ttu-id="937ab-152">Pro klienta C sady SDK je certifikát umístěný ve složce "\\c\\certifikátů" v kořenovém úložišti.</span><span class="sxs-lookup"><span data-stu-id="937ab-152">For the C client SDK the certificate is located under the folder "\\c\\certs" under the root of the repo.</span></span> <span data-ttu-id="937ab-153">I když tyto kořenových certifikátů je dlouhodobé, jsou stále může vyprší nebo odvolat.</span><span class="sxs-lookup"><span data-stu-id="937ab-153">Though these root certificates are long-lived, they still may expire or be revoked.</span></span> <span data-ttu-id="937ab-154">Pokud neexistuje žádný způsob aktualizace certifikát v zařízení, nemusí být zařízení následně připojit ke službě IoT Hub (nebo jiné cloudové služby).</span><span class="sxs-lookup"><span data-stu-id="937ab-154">If there is no way of updating the certificate on the device, the device may not be able to subsequently connect to the IoT Hub (or any other cloud service).</span></span> <span data-ttu-id="937ab-155">Toto riziko bude efektivně snížilo s znamená aktualizovat kořenový certifikát po nasazení zařízení IoT.</span><span class="sxs-lookup"><span data-stu-id="937ab-155">Having a means to update the root certificate once the IoT device is deployed will effectively mitigate this risk.</span></span>

## <a name="securing-the-connection"></a><span data-ttu-id="937ab-156">Zabezpečení připojení</span><span class="sxs-lookup"><span data-stu-id="937ab-156">Securing the connection</span></span>
<span data-ttu-id="937ab-157">Připojení k Internetu mezi zařízení IoT a IoT Hub, je zabezpečena pomocí standardní zabezpečení TLS (Transport Layer).</span><span class="sxs-lookup"><span data-stu-id="937ab-157">Internet connection between the IoT device and IoT Hub is secured using the Transport Layer Security (TLS) standard.</span></span> <span data-ttu-id="937ab-158">Azure IoT podporuje [TLS 1.2][lnk-tls12], TLS 1.1 a TLS 1.0, v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="937ab-158">Azure IoT supports [TLS 1.2][lnk-tls12], TLS 1.1 and TLS 1.0, in this order.</span></span> <span data-ttu-id="937ab-159">Podpora pro protokol TLS 1.0 je k dispozici pouze z důvodů zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="937ab-159">Support for TLS 1.0 is provided for backward compatibility only.</span></span> <span data-ttu-id="937ab-160">Doporučujeme použít protokol TLS 1.2, protože poskytuje nejvyšší zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="937ab-160">It is recommended to use TLS 1.2 since it provides the most security.</span></span>

<span data-ttu-id="937ab-161">Azure IoT Suite podporuje následující šifrovací sady, v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="937ab-161">Azure IoT Suite supports the following Cipher Suites, in this order.</span></span>

| <span data-ttu-id="937ab-162">Šifrovací sada</span><span class="sxs-lookup"><span data-stu-id="937ab-162">Cipher Suite</span></span> | <span data-ttu-id="937ab-163">Délka</span><span class="sxs-lookup"><span data-stu-id="937ab-163">Length</span></span> |
| --- | --- |
| <span data-ttu-id="937ab-164">Protokol TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 secp384r1 ECDH (0xc028) (ekvalizéru</span><span class="sxs-lookup"><span data-stu-id="937ab-164">TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (eq.</span></span> <span data-ttu-id="937ab-165">FS 7680 bits RSA)</span><span class="sxs-lookup"><span data-stu-id="937ab-165">7680 bits RSA) FS</span></span> |<span data-ttu-id="937ab-166">256</span><span class="sxs-lookup"><span data-stu-id="937ab-166">256</span></span> |
| <span data-ttu-id="937ab-167">Protokol TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (ekvalizéru</span><span class="sxs-lookup"><span data-stu-id="937ab-167">TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (eq.</span></span> <span data-ttu-id="937ab-168">FS 3072 bits RSA)</span><span class="sxs-lookup"><span data-stu-id="937ab-168">3072 bits RSA) FS</span></span> |<span data-ttu-id="937ab-169">128</span><span class="sxs-lookup"><span data-stu-id="937ab-169">128</span></span> |
| <span data-ttu-id="937ab-170">Protokol TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (ekvalizéru</span><span class="sxs-lookup"><span data-stu-id="937ab-170">TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq.</span></span> <span data-ttu-id="937ab-171">FS 7680 bits RSA)</span><span class="sxs-lookup"><span data-stu-id="937ab-171">7680 bits RSA) FS</span></span> |<span data-ttu-id="937ab-172">256</span><span class="sxs-lookup"><span data-stu-id="937ab-172">256</span></span> |
| <span data-ttu-id="937ab-173">Protokol TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (ekvalizéru</span><span class="sxs-lookup"><span data-stu-id="937ab-173">TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq.</span></span> <span data-ttu-id="937ab-174">FS 3072 bits RSA)</span><span class="sxs-lookup"><span data-stu-id="937ab-174">3072 bits RSA) FS</span></span> |<span data-ttu-id="937ab-175">128</span><span class="sxs-lookup"><span data-stu-id="937ab-175">128</span></span> |
| <span data-ttu-id="937ab-176">Protokol TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d)</span><span class="sxs-lookup"><span data-stu-id="937ab-176">TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d)</span></span> |<span data-ttu-id="937ab-177">256</span><span class="sxs-lookup"><span data-stu-id="937ab-177">256</span></span> |
| <span data-ttu-id="937ab-178">Protokol TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256 (0x9c)</span><span class="sxs-lookup"><span data-stu-id="937ab-178">TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256 (0x9c)</span></span> |<span data-ttu-id="937ab-179">128</span><span class="sxs-lookup"><span data-stu-id="937ab-179">128</span></span> |
| <span data-ttu-id="937ab-180">Protokol TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256 (0x3d)</span><span class="sxs-lookup"><span data-stu-id="937ab-180">TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256 (0x3d)</span></span> |<span data-ttu-id="937ab-181">256</span><span class="sxs-lookup"><span data-stu-id="937ab-181">256</span></span> |
| <span data-ttu-id="937ab-182">Protokol TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0x3c)</span><span class="sxs-lookup"><span data-stu-id="937ab-182">TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0x3c)</span></span> |<span data-ttu-id="937ab-183">128</span><span class="sxs-lookup"><span data-stu-id="937ab-183">128</span></span> |
| <span data-ttu-id="937ab-184">Protokol TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA (0x35)</span><span class="sxs-lookup"><span data-stu-id="937ab-184">TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA (0x35)</span></span> |<span data-ttu-id="937ab-185">256</span><span class="sxs-lookup"><span data-stu-id="937ab-185">256</span></span> |
| <span data-ttu-id="937ab-186">Protokol TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f)</span><span class="sxs-lookup"><span data-stu-id="937ab-186">TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f)</span></span> |<span data-ttu-id="937ab-187">128</span><span class="sxs-lookup"><span data-stu-id="937ab-187">128</span></span> |
| <span data-ttu-id="937ab-188">Protokol TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA (0xa)</span><span class="sxs-lookup"><span data-stu-id="937ab-188">TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA (0xa)</span></span> |<span data-ttu-id="937ab-189">112</span><span class="sxs-lookup"><span data-stu-id="937ab-189">112</span></span> |

## <a name="securing-the-cloud"></a><span data-ttu-id="937ab-190">Zabezpečení cloudu</span><span class="sxs-lookup"><span data-stu-id="937ab-190">Securing the cloud</span></span>
<span data-ttu-id="937ab-191">Azure IoT Hub umožňuje definice [zásad řízení přístupu] [ lnk-protocols] pro každý klíč zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="937ab-191">Azure IoT Hub allows definition of [access control policies][lnk-protocols] for each security key.</span></span> <span data-ttu-id="937ab-192">Následující sadu oprávnění, používá k udělení přístupu k všechny koncové body centra IoT.</span><span class="sxs-lookup"><span data-stu-id="937ab-192">It uses the following set of permissions to grant access to each of IoT Hub's endpoints.</span></span> <span data-ttu-id="937ab-193">Oprávnění omezit přístup do služby IoT Hub, v závislosti na funkcích.</span><span class="sxs-lookup"><span data-stu-id="937ab-193">Permissions limit the access to an IoT Hub based on functionality.</span></span>

* <span data-ttu-id="937ab-194">**RegistryRead**.</span><span class="sxs-lookup"><span data-stu-id="937ab-194">**RegistryRead**.</span></span> <span data-ttu-id="937ab-195">Uděluje přístup do registru identit pro čtení.</span><span class="sxs-lookup"><span data-stu-id="937ab-195">Grants read access to the identity registry.</span></span> <span data-ttu-id="937ab-196">Další informace najdete v tématu [registru identit][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="937ab-196">For more information, see [identity registry][lnk-identity-registry].</span></span>
* <span data-ttu-id="937ab-197">**RegistryReadWrite**.</span><span class="sxs-lookup"><span data-stu-id="937ab-197">**RegistryReadWrite**.</span></span> <span data-ttu-id="937ab-198">Uděluje přístup čtení a zápisu do registru identit.</span><span class="sxs-lookup"><span data-stu-id="937ab-198">Grants read and write access to the identity registry.</span></span> <span data-ttu-id="937ab-199">Další informace najdete v tématu [registru identit][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="937ab-199">For more information, see [identity registry][lnk-identity-registry].</span></span>
* <span data-ttu-id="937ab-200">**ServiceConnect**.</span><span class="sxs-lookup"><span data-stu-id="937ab-200">**ServiceConnect**.</span></span> <span data-ttu-id="937ab-201">Uděluje přístup do cloudu komunikace a sledování koncových bodů služby přístupem.</span><span class="sxs-lookup"><span data-stu-id="937ab-201">Grants access to cloud service-facing communication and monitoring endpoints.</span></span> <span data-ttu-id="937ab-202">Například uděluje oprávnění k back-end cloudové služby na příjem zpráv typu zařízení cloud, odesílání zpráv typu cloud zařízení a načíst odpovídající potvrzování doručení.</span><span class="sxs-lookup"><span data-stu-id="937ab-202">For example, it grants permission to back-end cloud services to receive device-to-cloud messages, send cloud-to-device messages, and retrieve the corresponding delivery acknowledgments.</span></span>
* <span data-ttu-id="937ab-203">**DeviceConnect**.</span><span class="sxs-lookup"><span data-stu-id="937ab-203">**DeviceConnect**.</span></span> <span data-ttu-id="937ab-204">Uděluje přístup k zařízení přístupem koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="937ab-204">Grants access to device-facing endpoints.</span></span> <span data-ttu-id="937ab-205">Například uděluje oprávnění k odesílání zpráv typu zařízení cloud a příjem zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="937ab-205">For example, it grants permission to send device-to-cloud messages and receive cloud-to-device messages.</span></span> <span data-ttu-id="937ab-206">Toto oprávnění je používán zařízení.</span><span class="sxs-lookup"><span data-stu-id="937ab-206">This permission is used by devices.</span></span>

<span data-ttu-id="937ab-207">Existují dva způsoby, jak získat **DeviceConnect** oprávnění službou IoT Hub s [tokeny zabezpečení][lnk-sas-tokens]: pomocí klíče identity zařízení nebo sdílený přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="937ab-207">There are two ways to obtain **DeviceConnect** permissions with IoT Hub with [security tokens][lnk-sas-tokens]: using a device identity key, or a shared access key.</span></span> <span data-ttu-id="937ab-208">Kromě toho je důležité si uvědomit, že všechny funkce, které jsou přístupné ze zařízení je zveřejněný prostřednictvím návrhu na koncové body s předponou `/devices/{deviceId}`.</span><span class="sxs-lookup"><span data-stu-id="937ab-208">Moreover, it is important to note that all functionality accessible from devices is exposed by design on endpoints with prefix `/devices/{deviceId}`.</span></span>

<span data-ttu-id="937ab-209">[Součásti služby můžete generovat jenom tokeny zabezpečení] [ lnk-service-tokens] pomocí sdílené zásady přístupu udělení příslušných oprávnění.</span><span class="sxs-lookup"><span data-stu-id="937ab-209">[Service components can only generate security tokens][lnk-service-tokens] using shared access policies granting the appropriate permissions.</span></span>

<span data-ttu-id="937ab-210">Azure IoT Hub a dalším službám, které může být součástí řešení povolit správu uživatelů pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="937ab-210">Azure IoT Hub and other services which may be part of the solution allow management of users using the Azure Active Directory.</span></span>

<span data-ttu-id="937ab-211">Data ve službě Azure IoT Hub požity mohou být spotřebovávána různých služeb, jako je Azure Stream Analytics a úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="937ab-211">Data ingested by Azure IoT Hub can be consumed by a variety of services such as Azure Stream Analytics and Azure blob storage.</span></span> <span data-ttu-id="937ab-212">Tyto služby umožňují přístup pro správu.</span><span class="sxs-lookup"><span data-stu-id="937ab-212">These services allow management access.</span></span> <span data-ttu-id="937ab-213">Další informace o těchto službách a k dispozici následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="937ab-213">Read more about these services and available options below:</span></span>

* <span data-ttu-id="937ab-214">[Azure Cosmos DB][lnk-docdb]: škálovatelné a plně indexované databázová služba pro částečně strukturovaných dat, který spravuje metadata pro zařízení, zřídíte, jako je například atributy, konfiguraci a vlastnosti zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="937ab-214">[Azure Cosmos DB][lnk-docdb]: A scalable, fully-indexed database service for semi-structured data that manages metadata for the devices you provision, such as attributes, configuration, and security properties.</span></span> <span data-ttu-id="937ab-215">Cosmos DB nabízí vysoce výkonné a vysokou propustností zpracování, bez ohledu na schéma indexování dat a bohaté rozhraní SQL.</span><span class="sxs-lookup"><span data-stu-id="937ab-215">Cosmos DB offers high-performance and high-throughput processing, schema-agnostic indexing of data, and a rich SQL query interface.</span></span>
* <span data-ttu-id="937ab-216">[Azure Stream Analytics][lnk-asa]: zpracování v cloudu, která umožňuje rychle vyvíjet a nasadit řešení analytics nízkonákladové k odhalení přehledy v reálném čase ze zařízení, senzorů, infrastruktura, streamu v reálném čase a aplikace.</span><span class="sxs-lookup"><span data-stu-id="937ab-216">[Azure Stream Analytics][lnk-asa]: Real-time stream processing in the cloud that enables you to rapidly develop and deploy a low-cost analytics solution to uncover real-time insights from devices, sensors, infrastructure, and applications.</span></span> <span data-ttu-id="937ab-217">Data z této plně spravovanou službu, můžete škálovat na jakýkoli svazek, zatímco stále dosahuje vysoké propustnosti, s nízkou latencí a odolnost proti chybám.</span><span class="sxs-lookup"><span data-stu-id="937ab-217">The data from this fully-managed service can scale to any volume while still achieving high throughput, low latency, and resiliency.</span></span>
* <span data-ttu-id="937ab-218">[Azure App Services][lnk-appservices]: Cloudová platforma vytvářet výkonné webové a mobilní aplikace, které se připojují k datům odkudkoli; v cloudu nebo místně.</span><span class="sxs-lookup"><span data-stu-id="937ab-218">[Azure App Services][lnk-appservices]: A cloud platform to build powerful web and mobile apps that connect to data anywhere; in the cloud or on-premises.</span></span> <span data-ttu-id="937ab-219">Vytvářejte poutavé mobilní aplikace pro iOS, Android a Windows.</span><span class="sxs-lookup"><span data-stu-id="937ab-219">Build engaging mobile apps for iOS, Android, and Windows.</span></span> <span data-ttu-id="937ab-220">Integrate váš Software jako služba (SaaS) a podnikové aplikace s připojením se na pole k desítek cloudové služby a podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="937ab-220">Integrate with your Software as a Service (SaaS) and enterprise applications with out-of-the-box connectivity to dozens of cloud-based services and enterprise applications.</span></span> <span data-ttu-id="937ab-221">Kód na váš oblíbený jazyk a IDE (.NET, Node.js, PHP, Python nebo Java) k vytvoření webové aplikace a rozhraní API rychleji než kdy dřív.</span><span class="sxs-lookup"><span data-stu-id="937ab-221">Code in your favorite language and IDE (.NET, Node.js, PHP, Python, or Java) to build web apps and APIs faster than ever.</span></span>
* <span data-ttu-id="937ab-222">[Služba Logic Apps][lnk-logicapps]: funkce The Logic Apps služby Azure App Service pomáhá integrovat řešení IoT tak, aby vaše stávající-obchodní systémy a automatizovat pracovní postupy.</span><span class="sxs-lookup"><span data-stu-id="937ab-222">[Logic Apps][lnk-logicapps]: The Logic Apps feature of Azure App Service helps integrate your IoT solution to your existing line-of-business systems and automate workflow processes.</span></span> <span data-ttu-id="937ab-223">Služba Logic Apps umožňuje vývojářům navrhovat pracovní postupy, které začínají spouštěčem událostí a provádějí sérii kroků – pravidla a akce, které použít Výkonné konektory pro integraci s procesy vaší firmy.</span><span class="sxs-lookup"><span data-stu-id="937ab-223">Logic Apps enables developers to design workflows that start from a trigger and then execute a series of steps—rules and actions that use powerful connectors to integrate with your business processes.</span></span> <span data-ttu-id="937ab-224">Služba Logic Apps nabízí připojení se na pole k velká ekosystém SaaS, cloudových a místních aplikací.</span><span class="sxs-lookup"><span data-stu-id="937ab-224">Logic Apps offers out-of-the-box connectivity to a vast ecosystem of SaaS, cloud-based, and on-premises applications.</span></span>
* <span data-ttu-id="937ab-225">[Úložiště objektů blob Azure][lnk-blob]: spolehlivé a ekonomické cloudové úložiště pro data, která vaše zařízení odesílají do cloudu.</span><span class="sxs-lookup"><span data-stu-id="937ab-225">[Azure blob storage][lnk-blob]: Reliable, economical cloud storage for the data that your devices send to the cloud.</span></span>

## <a name="conclusion"></a><span data-ttu-id="937ab-226">Závěr</span><span class="sxs-lookup"><span data-stu-id="937ab-226">Conclusion</span></span>
<span data-ttu-id="937ab-227">Tento článek obsahuje přehled implementace úroveň podrobností pro navrhování a nasazení infrastruktury IoT pomocí Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="937ab-227">This article provides overview of implementation level details for designing and deploying an IoT infrastructure using Azure IoT.</span></span> <span data-ttu-id="937ab-228">Konfigurace jednotlivých součástí zabezpečená je klíč v zabezpečení celkové infrastruktury IoT.</span><span class="sxs-lookup"><span data-stu-id="937ab-228">Configuring each component to be secure is key in securing the overall IoT infrastructure.</span></span> <span data-ttu-id="937ab-229">K dispozici v Azure IoT volbách návrhu zadejte určité úrovně flexibilitu a možnost výběru; Každý výběr však může mít vliv na zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="937ab-229">The design choices available in Azure IoT provide some level of flexibility and choice; however, each choice may have security implications.</span></span> <span data-ttu-id="937ab-230">Doporučuje se každý z těchto možností vyhodnotí vyhodnoťte riziko/náklady.</span><span class="sxs-lookup"><span data-stu-id="937ab-230">It is recommended that each of these choices be evaluated through a risk/cost assessment.</span></span>

## <a name="see-also"></a><span data-ttu-id="937ab-231">Viz také</span><span class="sxs-lookup"><span data-stu-id="937ab-231">See also</span></span>
<span data-ttu-id="937ab-232">Můžete si taky prostudovat některé další funkce a možnosti předkonfigurovaných řešení sady IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="937ab-232">You can also explore some of the other features and capabilities of the IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="937ab-233">[Přehled řešení předkonfigurované prediktivní údržby][lnk-predictive-overview]</span><span class="sxs-lookup"><span data-stu-id="937ab-233">[Predictive maintenance preconfigured solution overview][lnk-predictive-overview]</span></span>
* <span data-ttu-id="937ab-234">[Nejčastější dotazy k sadě IoT Suite][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="937ab-234">[Frequently asked questions for IoT Suite][lnk-faq]</span></span>

<span data-ttu-id="937ab-235">Další informace o zabezpečení služby IoT Hub v [řízení přístupu ke službě IoT Hub] [ lnk-devguide-security] v příručce pro vývojáře IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="937ab-235">You can read about IoT Hub security in [Control access to IoT Hub][lnk-devguide-security] in the IoT Hub developer guide.</span></span>


[img-overview]: media/iot-suite-security-deployment/overview.png

[lnk-security-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../iot-hub/iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../iot-hub/iot-hub-devguide-security.md#use-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md