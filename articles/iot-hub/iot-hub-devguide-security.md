---
title: "Porozumět zabezpečení služby Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře – řízení přístupu ke službě IoT Hub pro aplikace pro zařízení a back-end aplikace. Obsahuje informace o tokeny zabezpečení a podporu pro certifikáty X.509."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 45631e70-865b-4e06-bb1d-aae1175a52ba
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: e4fe5400ffcf4446392015aada031dd4dfbf238a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="control-access-to-iot-hub"></a><span data-ttu-id="64528-104">Řízení přístupu k IoT Hubu</span><span class="sxs-lookup"><span data-stu-id="64528-104">Control access to IoT Hub</span></span>

<span data-ttu-id="64528-105">Tento článek popisuje možnosti pro zabezpečení služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="64528-105">This article describes the options for securing your IoT hub.</span></span> <span data-ttu-id="64528-106">IoT Hub používá *oprávnění* k udělení přístupu k každý koncový bod centra IoT.</span><span class="sxs-lookup"><span data-stu-id="64528-106">IoT Hub uses *permissions* to grant access to each IoT hub endpoint.</span></span> <span data-ttu-id="64528-107">Oprávnění omezit přístup do služby IoT hub, v závislosti na funkcích.</span><span class="sxs-lookup"><span data-stu-id="64528-107">Permissions limit the access to an IoT hub based on functionality.</span></span>

<span data-ttu-id="64528-108">Tento článek popisuje:</span><span class="sxs-lookup"><span data-stu-id="64528-108">This article describes:</span></span>

* <span data-ttu-id="64528-109">Jiná oprávnění, že můžete udělit do zařízení nebo back-end aplikace pro přístup k službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="64528-109">The different permissions that you can grant to a device or back-end app to access your IoT hub.</span></span>
* <span data-ttu-id="64528-110">Proces ověřování a tokenů se používá k ověření oprávnění.</span><span class="sxs-lookup"><span data-stu-id="64528-110">The authentication process and the tokens it uses to verify permissions.</span></span>
* <span data-ttu-id="64528-111">Postup určení oboru přihlašovací údaje k omezení přístupu ke konkrétním prostředkům.</span><span class="sxs-lookup"><span data-stu-id="64528-111">How to scope credentials to limit access to specific resources.</span></span>
* <span data-ttu-id="64528-112">Podpora služby IoT Hub pro certifikáty X.509.</span><span class="sxs-lookup"><span data-stu-id="64528-112">IoT Hub support for X.509 certificates.</span></span>
* <span data-ttu-id="64528-113">Mechanismy ověřování vlastní zařízení, které používají existující registrech identity zařízení nebo schémat ověřování.</span><span class="sxs-lookup"><span data-stu-id="64528-113">Custom device authentication mechanisms that use existing device identity registries or authentication schemes.</span></span>

### <a name="when-to-use"></a><span data-ttu-id="64528-114">Kdy je použít</span><span class="sxs-lookup"><span data-stu-id="64528-114">When to use</span></span>

<span data-ttu-id="64528-115">Musí mít příslušná oprávnění pro přístup k libovolnému koncové body centra IoT.</span><span class="sxs-lookup"><span data-stu-id="64528-115">You must have appropriate permissions to access any of the IoT Hub endpoints.</span></span> <span data-ttu-id="64528-116">Zařízení musí obsahovat třeba token obsahující zabezpečovací pověření společně s každou zprávu, kterou odešle do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="64528-116">For example, a device must include a token containing security credentials along with every message it sends to IoT Hub.</span></span>

## <a name="access-control-and-permissions"></a><span data-ttu-id="64528-117">Řízení přístupu a oprávnění</span><span class="sxs-lookup"><span data-stu-id="64528-117">Access control and permissions</span></span>

<span data-ttu-id="64528-118">Můžete udělit [oprávnění](#iot-hub-permissions) následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="64528-118">You can grant [permissions](#iot-hub-permissions) in the following ways:</span></span>

* <span data-ttu-id="64528-119">**IoT hub úrovni sdílených zásad přístupu**.</span><span class="sxs-lookup"><span data-stu-id="64528-119">**IoT hub-level shared access policies**.</span></span> <span data-ttu-id="64528-120">Zásady sdíleného přístupu můžete udělit libovolnou kombinaci [oprávnění](#iot-hub-permissions).</span><span class="sxs-lookup"><span data-stu-id="64528-120">Shared access policies can grant any combination of [permissions](#iot-hub-permissions).</span></span> <span data-ttu-id="64528-121">Můžete definovat zásady v [portál Azure][lnk-management-portal], nebo programově pomocí [zprostředkovatele prostředků služby IoT Hub rozhraní REST API][lnk-resource-provider-apis].</span><span class="sxs-lookup"><span data-stu-id="64528-121">You can define policies in the [Azure portal][lnk-management-portal], or programmatically by using the [IoT Hub resource provider REST APIs][lnk-resource-provider-apis].</span></span> <span data-ttu-id="64528-122">Nově vytvořený IoT hub má následující výchozích zásad:</span><span class="sxs-lookup"><span data-stu-id="64528-122">A newly created IoT hub has the following default policies:</span></span>

  * <span data-ttu-id="64528-123">**iothubowner**: zásada se všechna oprávnění.</span><span class="sxs-lookup"><span data-stu-id="64528-123">**iothubowner**: Policy with all permissions.</span></span>
  * <span data-ttu-id="64528-124">**Služba**: zásada se **ServiceConnect** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="64528-124">**service**: Policy with **ServiceConnect** permission.</span></span>
  * <span data-ttu-id="64528-125">**zařízení**: zásada se **DeviceConnect** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="64528-125">**device**: Policy with **DeviceConnect** permission.</span></span>
  * <span data-ttu-id="64528-126">**registryRead**: zásada se **RegistryRead** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="64528-126">**registryRead**: Policy with **RegistryRead** permission.</span></span>
  * <span data-ttu-id="64528-127">**registryReadWrite**: zásada se **RegistryRead** a RegistryWrite oprávnění.</span><span class="sxs-lookup"><span data-stu-id="64528-127">**registryReadWrite**: Policy with **RegistryRead** and RegistryWrite permissions.</span></span>
  * <span data-ttu-id="64528-128">**Podle zařízení zabezpečovací pověření**.</span><span class="sxs-lookup"><span data-stu-id="64528-128">**Per-device security credentials**.</span></span> <span data-ttu-id="64528-129">Každé centrum IoT obsahuje [registru identit][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="64528-129">Each IoT Hub contains an [identity registry][lnk-identity-registry].</span></span> <span data-ttu-id="64528-130">Pro každé zařízení v registru této identity můžete nakonfigurovat zabezpečovací pověření, která udělují **DeviceConnect** oprávnění obor ke koncovým bodům odpovídající zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-130">For each device in this identity registry, you can configure security credentials that grant **DeviceConnect** permissions scoped to the corresponding device endpoints.</span></span>

<span data-ttu-id="64528-131">Například v typického řešení IoT:</span><span class="sxs-lookup"><span data-stu-id="64528-131">For example, in a typical IoT solution:</span></span>

* <span data-ttu-id="64528-132">Komponenty správy zařízení používá *registryReadWrite* zásad.</span><span class="sxs-lookup"><span data-stu-id="64528-132">The device management component uses the *registryReadWrite* policy.</span></span>
* <span data-ttu-id="64528-133">Součástí procesoru událostí se používá *služby* zásad.</span><span class="sxs-lookup"><span data-stu-id="64528-133">The event processor component uses the *service* policy.</span></span>
* <span data-ttu-id="64528-134">Používá komponentu spuštění zařízení obchodní logiku *služby* zásad.</span><span class="sxs-lookup"><span data-stu-id="64528-134">The run-time device business logic component uses the *service* policy.</span></span>
* <span data-ttu-id="64528-135">Jednotlivých zařízení se připojují přes přihlašovací údaje uložené v registru identit služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="64528-135">Individual devices connect using credentials stored in the IoT hub's identity registry.</span></span>

> [!NOTE]
> <span data-ttu-id="64528-136">V tématu [oprávnění](#iot-hub-permissions) podrobné informace.</span><span class="sxs-lookup"><span data-stu-id="64528-136">See [permissions](#iot-hub-permissions) for detailed information.</span></span>

## <a name="authentication"></a><span data-ttu-id="64528-137">Authentication</span><span class="sxs-lookup"><span data-stu-id="64528-137">Authentication</span></span>

<span data-ttu-id="64528-138">Azure IoT Hub uděluje přístup ke koncovým bodům kontrolou token proti zásady sdíleného přístupu a identit registru zabezpečovací pověření.</span><span class="sxs-lookup"><span data-stu-id="64528-138">Azure IoT Hub grants access to endpoints by verifying a token against the shared access policies and identity registry security credentials.</span></span>

<span data-ttu-id="64528-139">Zabezpečovací přihlašovací údaje, jako jsou symetrického klíče, se nikdy odeslány prostřednictvím sítě.</span><span class="sxs-lookup"><span data-stu-id="64528-139">Security credentials, such as symmetric keys, are never sent over the wire.</span></span>

> [!NOTE]
> <span data-ttu-id="64528-140">Poskytovatel prostředků Azure IoT Hub je zabezpečená vašeho předplatného Azure, jsou všechny zprostředkovatele v [Azure Resource Manager][lnk-azure-resource-manager].</span><span class="sxs-lookup"><span data-stu-id="64528-140">The Azure IoT Hub resource provider is secured through your Azure subscription, as are all providers in the [Azure Resource Manager][lnk-azure-resource-manager].</span></span>

<span data-ttu-id="64528-141">Další informace o tom, jak vytvořit a používat tokeny zabezpečení najdete v tématu [tokeny zabezpečení služby IoT Hub][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="64528-141">For more information about how to construct and use security tokens, see [IoT Hub security tokens][lnk-sas-tokens].</span></span>

### <a name="protocol-specifics"></a><span data-ttu-id="64528-142">Protokol podrobností</span><span class="sxs-lookup"><span data-stu-id="64528-142">Protocol specifics</span></span>

<span data-ttu-id="64528-143">Každý podporovaný protokol, například MQTT, AMQP a HTTP, je určena k přenosu tokeny různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="64528-143">Each supported protocol, such as MQTT, AMQP, and HTTP, transports tokens in different ways.</span></span>

<span data-ttu-id="64528-144">Při použití MQTT, připojení paketu má ID zařízení jako ClientId, {iothubhostname} / {deviceId} v poli uživatelské jméno a do pole pro heslo tokenu SAS.</span><span class="sxs-lookup"><span data-stu-id="64528-144">When using MQTT, the CONNECT packet has the deviceId as the ClientId, {iothubhostname}/{deviceId} in the Username field, and a SAS token in the Password field.</span></span> <span data-ttu-id="64528-145">{iothubhostname} by měla být úplná CName služby IoT hub (například contoso.azure-devices.net).</span><span class="sxs-lookup"><span data-stu-id="64528-145">{iothubhostname} should be the full CName of the IoT hub (for example, contoso.azure-devices.net).</span></span>

<span data-ttu-id="64528-146">Při použití [AMQP][lnk-amqp], podporuje IoT Hub [SASL prostý] [ lnk-sasl-plain] a [AMQP deklarace identity – zabezpečení na základě-] [ lnk-cbs].</span><span class="sxs-lookup"><span data-stu-id="64528-146">When using [AMQP][lnk-amqp], IoT Hub supports [SASL PLAIN][lnk-sasl-plain] and [AMQP Claims-Based-Security][lnk-cbs].</span></span>

<span data-ttu-id="64528-147">Pokud používáte AMQP deklarace identity – zabezpečení na základě-, standardní Určuje, jak přenést tyto tokeny.</span><span class="sxs-lookup"><span data-stu-id="64528-147">If you use AMQP claims-based-security, the standard specifies how to transmit these tokens.</span></span>

<span data-ttu-id="64528-148">Pro SASL prostý **uživatelské jméno** může být:</span><span class="sxs-lookup"><span data-stu-id="64528-148">For SASL PLAIN, the **username** can be:</span></span>

* <span data-ttu-id="64528-149">`{policyName}@sas.root.{iothubName}`Pokud se používá tokeny úrovni centra IoT.</span><span class="sxs-lookup"><span data-stu-id="64528-149">`{policyName}@sas.root.{iothubName}` if using IoT hub-level tokens.</span></span>
* <span data-ttu-id="64528-150">`{deviceId}@sas.{iothubname}`Pokud používáte zařízení obor tokeny.</span><span class="sxs-lookup"><span data-stu-id="64528-150">`{deviceId}@sas.{iothubname}` if using device-scoped tokens.</span></span>

<span data-ttu-id="64528-151">V obou případech pole pro heslo obsahuje token, jak je popsáno v [tokeny zabezpečení služby IoT Hub][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="64528-151">In both cases, the password field contains the token, as described in [IoT Hub security tokens][lnk-sas-tokens].</span></span>

<span data-ttu-id="64528-152">HTTP implementuje ověřování zahrnutím platný token v **autorizace** hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="64528-152">HTTP implements authentication by including a valid token in the **Authorization** request header.</span></span>

#### <a name="example"></a><span data-ttu-id="64528-153">Příklad</span><span class="sxs-lookup"><span data-stu-id="64528-153">Example</span></span>

<span data-ttu-id="64528-154">Uživatelské jméno (DeviceId je malá a velká písmena):`iothubname.azure-devices.net/DeviceId`</span><span class="sxs-lookup"><span data-stu-id="64528-154">Username (DeviceId is case-sensitive): `iothubname.azure-devices.net/DeviceId`</span></span>

<span data-ttu-id="64528-155">Heslo (Generovat SAS token s [explorer zařízení] [ lnk-device-explorer] nástroj):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`</span><span class="sxs-lookup"><span data-stu-id="64528-155">Password (Generate SAS token with the [device explorer][lnk-device-explorer] tool): `SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`</span></span>

> [!NOTE]
> <span data-ttu-id="64528-156">[SDK služby Azure IoT] [ lnk-sdks] automaticky generovat tokeny při připojování ke službě.</span><span class="sxs-lookup"><span data-stu-id="64528-156">The [Azure IoT SDKs][lnk-sdks] automatically generate tokens when connecting to the service.</span></span> <span data-ttu-id="64528-157">V některých případech nepodporují SDK služby Azure IoT všechny protokoly nebo všechny metody ověřování.</span><span class="sxs-lookup"><span data-stu-id="64528-157">In some cases, the Azure IoT SDKs do not support all the protocols or all the authentication methods.</span></span>

### <a name="special-considerations-for-sasl-plain"></a><span data-ttu-id="64528-158">Zvláštní upozornění pro SASL prostý</span><span class="sxs-lookup"><span data-stu-id="64528-158">Special considerations for SASL PLAIN</span></span>

<span data-ttu-id="64528-159">Při použití SASL prostý s AMQP, klient připojení do služby IoT hub můžete použít jeden token pro každé připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="64528-159">When using SASL PLAIN with AMQP, a client connecting to an IoT hub can use a single token for each TCP connection.</span></span> <span data-ttu-id="64528-160">Když vyprší platnost tokenu, připojení TCP odpojí od služby a aktivuje o obnovení.</span><span class="sxs-lookup"><span data-stu-id="64528-160">When the token expires, the TCP connection disconnects from the service and triggers a reconnect.</span></span> <span data-ttu-id="64528-161">Toto chování, když není problematické pro back-end aplikačním je poškození pro aplikace na zařízení z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="64528-161">This behavior, while not problematic for a back-end app, is damaging for a device app for the following reasons:</span></span>

* <span data-ttu-id="64528-162">Brány obvykle jménem mnoho zařízení připojit.</span><span class="sxs-lookup"><span data-stu-id="64528-162">Gateways usually connect on behalf of many devices.</span></span> <span data-ttu-id="64528-163">Pokud používáte SASL prostý, mají vytvořit odlišné připojení protokolu TCP pro každé zařízení připojení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="64528-163">When using SASL PLAIN, they have to create a distinct TCP connection for each device connecting to an IoT hub.</span></span> <span data-ttu-id="64528-164">Tento scénář také výrazně zvyšuje spotřeby energie a síťové prostředky a latence každé připojení zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-164">This scenario considerably increases the consumption of power and networking resources, and increases the latency of each device connection.</span></span>
* <span data-ttu-id="64528-165">Zařízení s omezenými zdroji jsou nevyhovělo vyšší využití prostředků se znovu připojit po každém vypršení platnosti tokenu.</span><span class="sxs-lookup"><span data-stu-id="64528-165">Resource-constrained devices are adversely affected by the increased use of resources to reconnect after each token expiration.</span></span>

## <a name="scope-iot-hub-level-credentials"></a><span data-ttu-id="64528-166">Přihlašovací údaje úrovni centra IoT oboru</span><span class="sxs-lookup"><span data-stu-id="64528-166">Scope IoT hub-level credentials</span></span>

<span data-ttu-id="64528-167">Můžete určit obor zásad zabezpečení na úrovni centra IoT tak, že vytvoříte tokeny pomocí identifikátoru URI prostředku s omezeným přístupem.</span><span class="sxs-lookup"><span data-stu-id="64528-167">You can scope IoT hub-level security policies by creating tokens with a restricted resource URI.</span></span> <span data-ttu-id="64528-168">Koncový bod k odesílání zpráv typu zařízení cloud ze zařízení, je třeba **/devices/ {deviceId} / zprávy/události**.</span><span class="sxs-lookup"><span data-stu-id="64528-168">For example, the endpoint to send device-to-cloud messages from a device is **/devices/{deviceId}/messages/events**.</span></span> <span data-ttu-id="64528-169">Můžete taky zásady sdíleného přístupu úrovně rozbočovače IoT s **DeviceConnect** oprávnění pro přihlášení token je jejichž resourceURI **/devices/ {deviceId}**.</span><span class="sxs-lookup"><span data-stu-id="64528-169">You can also use an IoT hub-level shared access policy with **DeviceConnect** permissions to sign a token whose resourceURI is **/devices/{deviceId}**.</span></span> <span data-ttu-id="64528-170">Tento postup vytvoří token, který lze použít k odeslání zprávy jménem zařízení pouze **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="64528-170">This approach creates a token that is only usable to send messages on behalf of device **deviceId**.</span></span>

<span data-ttu-id="64528-171">Tento mechanismus je podobná [zásad vydavatele Event Hubs][lnk-event-hubs-publisher-policy]a umožňuje implementovat vlastní metody ověřování.</span><span class="sxs-lookup"><span data-stu-id="64528-171">This mechanism is similar to the [Event Hubs publisher policy][lnk-event-hubs-publisher-policy], and enables you to implement custom authentication methods.</span></span>

## <a name="security-tokens"></a><span data-ttu-id="64528-172">Tokeny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="64528-172">Security tokens</span></span>

<span data-ttu-id="64528-173">IoT Hub používá tokeny zabezpečení k ověřování zařízení a služby se odesílání klíče v drátové síti.</span><span class="sxs-lookup"><span data-stu-id="64528-173">IoT Hub uses security tokens to authenticate devices and services to avoid sending keys on the wire.</span></span> <span data-ttu-id="64528-174">Kromě toho mají omezenou dobu platnosti a obor tokeny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="64528-174">Additionally, security tokens are limited in time validity and scope.</span></span> <span data-ttu-id="64528-175">[Sady SDK služby Azure IoT] [ lnk-sdks] automaticky generovat tokeny bez nutnosti žádnou zvláštní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="64528-175">[Azure IoT SDKs][lnk-sdks] automatically generate tokens without requiring any special configuration.</span></span> <span data-ttu-id="64528-176">Některé scénáře vyžadují, abyste generovat a používat přímo tokeny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="64528-176">Some scenarios do require you to generate and use security tokens directly.</span></span> <span data-ttu-id="64528-177">Mezi tyto scénáře patří:</span><span class="sxs-lookup"><span data-stu-id="64528-177">Such scenarios include:</span></span>

* <span data-ttu-id="64528-178">Přímé použití ploch MQTT, AMQP a HTTP.</span><span class="sxs-lookup"><span data-stu-id="64528-178">The direct use of the MQTT, AMQP, or HTTP surfaces.</span></span>
* <span data-ttu-id="64528-179">Implementace vzoru služba tokenu, jak je popsáno v [ověřování zařízení vlastní][lnk-custom-auth].</span><span class="sxs-lookup"><span data-stu-id="64528-179">The implementation of the token service pattern, as explained in [Custom device authentication][lnk-custom-auth].</span></span>

<span data-ttu-id="64528-180">Centrum IoT také umožňuje zařízení k ověření službou IoT Hub pomocí [certifikáty X.509][lnk-x509].</span><span class="sxs-lookup"><span data-stu-id="64528-180">IoT Hub also allows devices to authenticate with IoT Hub using [X.509 certificates][lnk-x509].</span></span>

### <a name="security-token-structure"></a><span data-ttu-id="64528-181">Struktura tokenu zabezpečení</span><span class="sxs-lookup"><span data-stu-id="64528-181">Security token structure</span></span>

<span data-ttu-id="64528-182">Pomocí tokenů zabezpečení udělte přístup k zařízením s ohraničenou čas a služeb na určité funkce IoT hub.</span><span class="sxs-lookup"><span data-stu-id="64528-182">You use security tokens to grant time-bounded access to devices and services to specific functionality in IoT Hub.</span></span> <span data-ttu-id="64528-183">Získat autorizaci k připojení ke službě IoT Hub, zařízení a služby, musí poslat tokeny zabezpečení, které jsou podepsané sdíleného přístupu nebo symetrického klíče.</span><span class="sxs-lookup"><span data-stu-id="64528-183">To get authorization to connect to IoT Hub, devices and services must send security tokens signed with either a shared access or symmetric key.</span></span> <span data-ttu-id="64528-184">Tyto klíče jsou uloženy s identitu zařízení v registru identit.</span><span class="sxs-lookup"><span data-stu-id="64528-184">These keys are stored with a device identity in the identity registry.</span></span>

<span data-ttu-id="64528-185">Token podepsán sdílený přístupový klíč uděluje přístup k všechny funkce související s oprávněními zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="64528-185">A token signed with a shared access key grants access to all the functionality associated with the shared access policy permissions.</span></span> <span data-ttu-id="64528-186">Podepsaný token pomocí identity zařízení symetrického klíče pouze uděluje **DeviceConnect** oprávnění pro identitu související zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-186">A token signed with a device identity's symmetric key only grants the **DeviceConnect** permission for the associated device identity.</span></span>

<span data-ttu-id="64528-187">Token zabezpečení má následující formát:</span><span class="sxs-lookup"><span data-stu-id="64528-187">The security token has the following format:</span></span>

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

<span data-ttu-id="64528-188">Zde jsou očekávaných hodnot:</span><span class="sxs-lookup"><span data-stu-id="64528-188">Here are the expected values:</span></span>

| <span data-ttu-id="64528-189">Hodnota</span><span class="sxs-lookup"><span data-stu-id="64528-189">Value</span></span> | <span data-ttu-id="64528-190">Popis</span><span class="sxs-lookup"><span data-stu-id="64528-190">Description</span></span> |
| --- | --- |
| <span data-ttu-id="64528-191">{podpis}</span><span class="sxs-lookup"><span data-stu-id="64528-191">{signature}</span></span> |<span data-ttu-id="64528-192">Řetězec podpisu HMAC SHA256 ve tvaru: `{URL-encoded-resourceURI} + "\n" + expiry`.</span><span class="sxs-lookup"><span data-stu-id="64528-192">An HMAC-SHA256 signature string of the form: `{URL-encoded-resourceURI} + "\n" + expiry`.</span></span> <span data-ttu-id="64528-193">**Důležité**: klíč je dekódovat z formátu base64 a použít jako klíč, aby prováděly výpočty HMAC SHA256.</span><span class="sxs-lookup"><span data-stu-id="64528-193">**Important**: The key is decoded from base64 and used as key to perform the HMAC-SHA256 computation.</span></span> |
| <span data-ttu-id="64528-194">{resourceURI}</span><span class="sxs-lookup"><span data-stu-id="64528-194">{resourceURI}</span></span> |<span data-ttu-id="64528-195">Předpony identifikátoru URI (podle segmentu) koncových bodů, které lze přistupovat pomocí tohoto tokenu, počínaje název hostitele služby IoT hub (žádné protocol).</span><span class="sxs-lookup"><span data-stu-id="64528-195">URI prefix (by segment) of the endpoints that can be accessed with this token, starting with host name of the IoT hub (no protocol).</span></span> <span data-ttu-id="64528-196">Například `myHub.azure-devices.net/devices/device1`.</span><span class="sxs-lookup"><span data-stu-id="64528-196">For example, `myHub.azure-devices.net/devices/device1`</span></span> |
| <span data-ttu-id="64528-197">{vypršení platnosti}</span><span class="sxs-lookup"><span data-stu-id="64528-197">{expiry}</span></span> |<span data-ttu-id="64528-198">Řetězce UTF8 pro počet sekund od 00:00:00 UTC epoch na 1. ledna pod hodnotou 1970.</span><span class="sxs-lookup"><span data-stu-id="64528-198">UTF8 strings for number of seconds since the epoch 00:00:00 UTC on 1 January 1970.</span></span> |
| <span data-ttu-id="64528-199">{Adresu URL-kódovaný resourceURI}</span><span class="sxs-lookup"><span data-stu-id="64528-199">{URL-encoded-resourceURI}</span></span> |<span data-ttu-id="64528-200">Nižší případ kódování URL prostředku malá identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="64528-200">Lower case URL-encoding of the lower case resource URI</span></span> |
| <span data-ttu-id="64528-201">{policyName}</span><span class="sxs-lookup"><span data-stu-id="64528-201">{policyName}</span></span> |<span data-ttu-id="64528-202">Název zásady sdíleného přístupu, na který odkazuje tento token.</span><span class="sxs-lookup"><span data-stu-id="64528-202">The name of the shared access policy to which this token refers.</span></span> <span data-ttu-id="64528-203">Chybějící Pokud token odkazuje na přihlašovací údaje registru zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-203">Absent if the token refers to device-registry credentials.</span></span> |

<span data-ttu-id="64528-204">**Poznámka: na předponě**: předpony identifikátoru URI se počítá podle segmentu a ne serverem znak.</span><span class="sxs-lookup"><span data-stu-id="64528-204">**Note on prefix**: The URI prefix is computed by segment and not by character.</span></span> <span data-ttu-id="64528-205">Například `/a/b` předpona pro `/a/b/c` , ale ne pro `/a/bc`.</span><span class="sxs-lookup"><span data-stu-id="64528-205">For example `/a/b` is a prefix for `/a/b/c` but not for `/a/bc`.</span></span>

<span data-ttu-id="64528-206">Následující fragment kódu Node.js ukazuje funkci s názvem **generateSasToken** , vypočítá token zabezpečení ze vstupních údajů `resourceUri, signingKey, policyName, expiresInMins`.</span><span class="sxs-lookup"><span data-stu-id="64528-206">The following Node.js snippet shows a function called **generateSasToken** that computes the token from the inputs `resourceUri, signingKey, policyName, expiresInMins`.</span></span> <span data-ttu-id="64528-207">V dalších oddílech jsou upřesněny postupy k chybě při inicializaci jiné vstupy pro případy použití v odlišných tokenu.</span><span class="sxs-lookup"><span data-stu-id="64528-207">The next sections detail how to initialize the different inputs for the different token use cases.</span></span>

```nodejs
var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
    resourceUri = encodeURIComponent(resourceUri);

    // Set expiration in seconds
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    expires = Math.ceil(expires);
    var toSign = resourceUri + '\n' + expires;

    // Use crypto
    var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
    hmac.update(toSign);
    var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

    // Construct autorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
};
```

<span data-ttu-id="64528-208">Jako porovnání je ekvivalentní kód Python pro vygenerování tokenu zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="64528-208">As a comparison, the equivalent Python code to generate a security token is:</span></span>

```python
from base64 import b64encode, b64decode
from hashlib import sha256
from time import time
from urllib import quote_plus, urlencode
from hmac import HMAC

def generate_sas_token(uri, key, policy_name, expiry=3600):
    ttl = time() + expiry
    sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
    print sign_key
    signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

    rawtoken = {
        'sr' :  uri,
        'sig': signature,
        'se' : str(int(ttl))
    }

    if policy_name is not None:
        rawtoken['skn'] = policy_name

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

> [!NOTE]
> <span data-ttu-id="64528-209">Vzhledem k tomu, že doba platnosti tokenu se ověří na počítačích IoT Hub, musí být minimální odlišily v hodinách na počítač, který generuje token.</span><span class="sxs-lookup"><span data-stu-id="64528-209">Since the time validity of the token is validated on IoT Hub machines, the drift on the clock of the machine that generates the token must be minimal.</span></span>

### <a name="use-sas-tokens-in-a-device-app"></a><span data-ttu-id="64528-210">Použití tokeny SAS v aplikaci pro zařízení</span><span class="sxs-lookup"><span data-stu-id="64528-210">Use SAS tokens in a device app</span></span>

<span data-ttu-id="64528-211">Existují dva způsoby, jak získat **DeviceConnect** oprávnění službou IoT Hub s tokeny zabezpečení: pomocí [zařízení symetrického klíče z registru identit](#use-a-symmetric-key-in-the-identity-registry), nebo použijte [sdílený přístupový klíč](#use-a-shared-access-policy).</span><span class="sxs-lookup"><span data-stu-id="64528-211">There are two ways to obtain **DeviceConnect** permissions with IoT Hub with security tokens: use a [symmetric device key from the identity registry](#use-a-symmetric-key-in-the-identity-registry), or use a [shared access key](#use-a-shared-access-policy).</span></span>

<span data-ttu-id="64528-212">Mějte na paměti, že všechny funkce, které jsou přístupné ze zařízení je zveřejněný prostřednictvím návrhu na koncové body s předponou `/devices/{deviceId}`.</span><span class="sxs-lookup"><span data-stu-id="64528-212">Remember that all functionality accessible from devices is exposed by design on endpoints with prefix `/devices/{deviceId}`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="64528-213">Jediným způsobem, že IoT Hub ověřuje určité zařízení používá symetrický klíč identity zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-213">The only way that IoT Hub authenticates a specific device is using the device identity symmetric key.</span></span> <span data-ttu-id="64528-214">V případech, pokud zásady sdíleného přístupu slouží k přístupu funkce zařízení, musí řešení zvažte komponentu vydání tokenu zabezpečení důvěryhodné komponentu.</span><span class="sxs-lookup"><span data-stu-id="64528-214">In cases when a shared access policy is used to access device functionality, the solution must consider the component issuing the security token as a trusted subcomponent.</span></span>

<span data-ttu-id="64528-215">Zařízení přístupem koncových bodů jsou (bez ohledu na protokol):</span><span class="sxs-lookup"><span data-stu-id="64528-215">The device-facing endpoints are (irrespective of the protocol):</span></span>

| <span data-ttu-id="64528-216">Koncový bod</span><span class="sxs-lookup"><span data-stu-id="64528-216">Endpoint</span></span> | <span data-ttu-id="64528-217">Funkce</span><span class="sxs-lookup"><span data-stu-id="64528-217">Functionality</span></span> |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |<span data-ttu-id="64528-218">Odesílání zpráv typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="64528-218">Send device-to-cloud messages.</span></span> |
| `{iot hub host name}/devices/{deviceId}/devicebound` |<span data-ttu-id="64528-219">Příjem zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-219">Receive cloud-to-device messages.</span></span> |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a><span data-ttu-id="64528-220">Použít symetrického klíče v registru identit</span><span class="sxs-lookup"><span data-stu-id="64528-220">Use a symmetric key in the identity registry</span></span>

<span data-ttu-id="64528-221">Při použití identitu zařízení symetrický klíč pro vygenerování tokenu, policyName (`skn`) element tokenu je vynechán.</span><span class="sxs-lookup"><span data-stu-id="64528-221">When using a device identity's symmetric key to generate a token, the policyName (`skn`) element of the token is omitted.</span></span>

<span data-ttu-id="64528-222">Například pro přístup ke všem funkcím zařízení vytvoří token musí mít následující parametry:</span><span class="sxs-lookup"><span data-stu-id="64528-222">For example, a token created to access all device functionality should have the following parameters:</span></span>

* <span data-ttu-id="64528-223">identifikátor URI prostředku: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span><span class="sxs-lookup"><span data-stu-id="64528-223">resource URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span></span>
* <span data-ttu-id="64528-224">podpisového klíče: pro všechny symetrický klíč `{device id}` identitu,</span><span class="sxs-lookup"><span data-stu-id="64528-224">signing key: any symmetric key for the `{device id}` identity,</span></span>
* <span data-ttu-id="64528-225">žádný název zásady</span><span class="sxs-lookup"><span data-stu-id="64528-225">no policy name,</span></span>
* <span data-ttu-id="64528-226">kdykoli vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="64528-226">any expiration time.</span></span>

<span data-ttu-id="64528-227">Příkladem pomocí funkce předchozí Node.js může být:</span><span class="sxs-lookup"><span data-stu-id="64528-227">An example using the preceding Node.js function would be:</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

<span data-ttu-id="64528-228">Výsledek, který uděluje přístup ke všem funkcím pro device1, bude:</span><span class="sxs-lookup"><span data-stu-id="64528-228">The result, which grants access to all functionality for device1, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> <span data-ttu-id="64528-229">Je možné vytvořit token SAS pomocí .NET [explorer zařízení] [ lnk-device-explorer] nástroje nebo a platformy, na základě uzlu [iothub-explorer] [ lnk-iothub-explorer]nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="64528-229">It is possible to generate a SAS token using the .NET [device explorer][lnk-device-explorer] tool or the cross-platform, node-based [iothub-explorer][lnk-iothub-explorer] command-line utility.</span></span>

### <a name="use-a-shared-access-policy"></a><span data-ttu-id="64528-230">Použijte zásady sdíleného přístupu</span><span class="sxs-lookup"><span data-stu-id="64528-230">Use a shared access policy</span></span>

<span data-ttu-id="64528-231">Při vytváření tokenu z zásady sdíleného přístupu, nastavení `skn` pole na název zásady.</span><span class="sxs-lookup"><span data-stu-id="64528-231">When you create a token from a shared access policy, set the `skn` field to the name of the policy.</span></span> <span data-ttu-id="64528-232">Tato zásada musí udělit **DeviceConnect** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="64528-232">This policy must grant the **DeviceConnect** permission.</span></span>

<span data-ttu-id="64528-233">Jsou dva základní scénáře pro přístup k zařízení funkce pomocí zásady sdíleného přístupu:</span><span class="sxs-lookup"><span data-stu-id="64528-233">The two main scenarios for using shared access policies to access device functionality are:</span></span>

* <span data-ttu-id="64528-234">[cloudové brány protokolu][lnk-endpoints],</span><span class="sxs-lookup"><span data-stu-id="64528-234">[cloud protocol gateways][lnk-endpoints],</span></span>
* <span data-ttu-id="64528-235">[token služby] [ lnk-custom-auth] použít k implementaci schémat vlastní ověřování.</span><span class="sxs-lookup"><span data-stu-id="64528-235">[token services][lnk-custom-auth] used to implement custom authentication schemes.</span></span>

<span data-ttu-id="64528-236">Vzhledem k tomu, že zásady sdíleného přístupu může potenciálně udělit přístup k připojení jako jakékoli zařízení, je důležité použít správný identifikátor URI, při vytváření tokenů zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="64528-236">Since the shared access policy can potentially grant access to connect as any device, it is important to use the correct resource URI when creating security tokens.</span></span> <span data-ttu-id="64528-237">Toto nastavení je obzvláště důležité pro token služby, které mají k určení rozsahu token k určitému zařízení pomocí identifikátoru URI prostředku.</span><span class="sxs-lookup"><span data-stu-id="64528-237">This setting is especially important for token services, which have to scope the token to a specific device using the resource URI.</span></span> <span data-ttu-id="64528-238">Tento bod je méně relevantní pro protokol brány jako jejich jsou již jehož prostřednictvím je umožněn přenos pro všechna zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-238">This point is less relevant for protocol gateways as they are already mediating traffic for all devices.</span></span>

<span data-ttu-id="64528-239">Jako příklad tokenu služby pomocí předem vytvořené sdílené zásady přístupu volat **zařízení** by vytvořit token s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="64528-239">As an example, a token service using the pre-created shared access policy called **device** would create a token with the following parameters:</span></span>

* <span data-ttu-id="64528-240">identifikátor URI prostředku: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span><span class="sxs-lookup"><span data-stu-id="64528-240">resource URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span></span>
* <span data-ttu-id="64528-241">podpisový klíč: jeden z klíčů `device` zásad</span><span class="sxs-lookup"><span data-stu-id="64528-241">signing key: one of the keys of the `device` policy,</span></span>
* <span data-ttu-id="64528-242">Název zásady: `device`,</span><span class="sxs-lookup"><span data-stu-id="64528-242">policy name: `device`,</span></span>
* <span data-ttu-id="64528-243">kdykoli vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="64528-243">any expiration time.</span></span>

<span data-ttu-id="64528-244">Příkladem pomocí funkce předchozí Node.js může být:</span><span class="sxs-lookup"><span data-stu-id="64528-244">An example using the preceding Node.js function would be:</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

<span data-ttu-id="64528-245">Výsledek, který uděluje přístup ke všem funkcím pro device1, bude:</span><span class="sxs-lookup"><span data-stu-id="64528-245">The result, which grants access to all functionality for device1, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

<span data-ttu-id="64528-246">Brána protokolu pro všechna zařízení, jednoduše nastavení identifikátoru URI prostředku použít stejný token do `myhub.azure-devices.net/devices`.</span><span class="sxs-lookup"><span data-stu-id="64528-246">A protocol gateway could use the same token for all devices simply setting the resource URI to `myhub.azure-devices.net/devices`.</span></span>

### <a name="use-security-tokens-from-service-components"></a><span data-ttu-id="64528-247">Použít tokeny zabezpečení ze součásti služby</span><span class="sxs-lookup"><span data-stu-id="64528-247">Use security tokens from service components</span></span>

<span data-ttu-id="64528-248">Součásti služby můžete vygenerovat pouze tokeny zabezpečení pomocí udělení příslušných oprávnění, jak je popsáno dříve zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="64528-248">Service components can only generate security tokens using shared access policies granting the appropriate permissions as explained previously.</span></span>

<span data-ttu-id="64528-249">Tady je funkce služby, které jsou zveřejněné na koncové body:</span><span class="sxs-lookup"><span data-stu-id="64528-249">Here is the service functions exposed on the endpoints:</span></span>

| <span data-ttu-id="64528-250">Koncový bod</span><span class="sxs-lookup"><span data-stu-id="64528-250">Endpoint</span></span> | <span data-ttu-id="64528-251">Funkce</span><span class="sxs-lookup"><span data-stu-id="64528-251">Functionality</span></span> |
| --- | --- |
| `{iot hub host name}/devices` |<span data-ttu-id="64528-252">Vytvoření, aktualizace, získání a odstranění identit zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-252">Create, update, retrieve, and delete device identities.</span></span> |
| `{iot hub host name}/messages/events` |<span data-ttu-id="64528-253">Příjem zpráv typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="64528-253">Receive device-to-cloud messages.</span></span> |
| `{iot hub host name}/servicebound/feedback` |<span data-ttu-id="64528-254">Přijímat zpětnou vazbu pro zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-254">Receive feedback for cloud-to-device messages.</span></span> |
| `{iot hub host name}/devicebound` |<span data-ttu-id="64528-255">Odesílání zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-255">Send cloud-to-device messages.</span></span> |

<span data-ttu-id="64528-256">Jako příklad služby generování použití předem vytvořené sdílené zásady přístupu volat **registryRead** by vytvořit token s následujícími parametry:</span><span class="sxs-lookup"><span data-stu-id="64528-256">As an example, a service generating using the pre-created shared access policy called **registryRead** would create a token with the following parameters:</span></span>

* <span data-ttu-id="64528-257">identifikátor URI prostředku: `{IoT hub name}.azure-devices.net/devices`,</span><span class="sxs-lookup"><span data-stu-id="64528-257">resource URI: `{IoT hub name}.azure-devices.net/devices`,</span></span>
* <span data-ttu-id="64528-258">podpisový klíč: jeden z klíčů `registryRead` zásad</span><span class="sxs-lookup"><span data-stu-id="64528-258">signing key: one of the keys of the `registryRead` policy,</span></span>
* <span data-ttu-id="64528-259">Název zásady: `registryRead`,</span><span class="sxs-lookup"><span data-stu-id="64528-259">policy name: `registryRead`,</span></span>
* <span data-ttu-id="64528-260">kdykoli vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="64528-260">any expiration time.</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

<span data-ttu-id="64528-261">Výsledek, který by udělit přístup ke čtení všech identit zařízení, bude:</span><span class="sxs-lookup"><span data-stu-id="64528-261">The result, which would grant access to read all device identities, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a><span data-ttu-id="64528-262">Podporované certifikáty X.509</span><span class="sxs-lookup"><span data-stu-id="64528-262">Supported X.509 certificates</span></span>

<span data-ttu-id="64528-263">K ověření zařízení IoT Hub můžete použít libovolný certifikát X.509.</span><span class="sxs-lookup"><span data-stu-id="64528-263">You can use any X.509 certificate to authenticate a device with IoT Hub.</span></span> <span data-ttu-id="64528-264">Certifikáty patří:</span><span class="sxs-lookup"><span data-stu-id="64528-264">Certificates include:</span></span>

* <span data-ttu-id="64528-265">**Existující certifikát X.509**.</span><span class="sxs-lookup"><span data-stu-id="64528-265">**An existing X.509 certificate**.</span></span> <span data-ttu-id="64528-266">Zařízení již pravděpodobně certifikát X.509 s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="64528-266">A device may already have an X.509 certificate associated with it.</span></span> <span data-ttu-id="64528-267">Zařízení můžete použít tento certifikát k ověření službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="64528-267">The device can use this certificate to authenticate with IoT Hub.</span></span>
* <span data-ttu-id="64528-268">**A samoobslužné generované a X-509 certifikátu podepsaného svým držitelem**.</span><span class="sxs-lookup"><span data-stu-id="64528-268">**A self-generated and self-signed X-509 certificate**.</span></span> <span data-ttu-id="64528-269">Výrobce zařízení nebo interní nástroje pro nasazení můžete generovat tyto certifikáty a uložení odpovídající privátní klíč (a certifikátu) na zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-269">A device manufacturer or in-house deployer can generate these certificates and store the corresponding private key (and certificate) on the device.</span></span> <span data-ttu-id="64528-270">Můžete použít nástroje, jako [OpenSSL] [ lnk-openssl] a [Windows SelfSignedCertificate] [ lnk-selfsigned] nástroj pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="64528-270">You can use tools such as [OpenSSL][lnk-openssl] and [Windows SelfSignedCertificate][lnk-selfsigned] utility for this purpose.</span></span>
* <span data-ttu-id="64528-271">**Certifikační Autority podepsané certifikát X.509**.</span><span class="sxs-lookup"><span data-stu-id="64528-271">**CA-signed X.509 certificate**.</span></span> <span data-ttu-id="64528-272">K identifikaci zařízení a ověřování službou IoT Hub, můžete certifikát X.509, generovány a podepsaný pomocí certifikační autority (CA).</span><span class="sxs-lookup"><span data-stu-id="64528-272">To identify a device and authenticate it with IoT Hub, you can use an X.509 certificate generated and signed by a Certification Authority (CA).</span></span> <span data-ttu-id="64528-273">IoT Hub pouze ověřuje, že kryptografický otisk uvedené shoduje má nakonfigurovaný kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="64528-273">IoT Hub only verifies that the thumbprint presented matches the configured thumbprint.</span></span> <span data-ttu-id="64528-274">IotHub nelze ověřit řetěz certifikátů.</span><span class="sxs-lookup"><span data-stu-id="64528-274">IotHub does not validate the certificate chain.</span></span>

<span data-ttu-id="64528-275">Zařízení může použít certifikát X.509 nebo token zabezpečení pro ověřování, ale ne obojí.</span><span class="sxs-lookup"><span data-stu-id="64528-275">A device may either use an X.509 certificate or a security token for authentication, but not both.</span></span>

### <a name="register-an-x509-certificate-for-a-device"></a><span data-ttu-id="64528-276">Registrovat certifikát X.509 pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-276">Register an X.509 certificate for a device</span></span>

<span data-ttu-id="64528-277">[Sady SDK služby Azure IoT pro jazyk C#] [ lnk-service-sdk] (verze 1.0.8+) podporuje registraci zařízení, které používá certifikátu X.509. certifikát pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="64528-277">The [Azure IoT Service SDK for C#][lnk-service-sdk] (version 1.0.8+) supports registering a device that uses an X.509 certificate for authentication.</span></span> <span data-ttu-id="64528-278">Jiná rozhraní API, jako je například importu a exportu zařízení také podporují certifikáty X.509.</span><span class="sxs-lookup"><span data-stu-id="64528-278">Other APIs such as import/export of devices also support X.509 certificates.</span></span>

### <a name="c-support"></a><span data-ttu-id="64528-279">C\# podpory</span><span class="sxs-lookup"><span data-stu-id="64528-279">C\# Support</span></span>

<span data-ttu-id="64528-280">**RegistryManager** třída poskytuje programový způsob, jak registrovat zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-280">The **RegistryManager** class provides a programmatic way to register a device.</span></span> <span data-ttu-id="64528-281">Konkrétně **AddDeviceAsync** a **UpdateDeviceAsync** metody umožňují zaregistrovat a aktualizujte zařízení v registru identit služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="64528-281">In particular, the **AddDeviceAsync** and **UpdateDeviceAsync** methods enable you to register and update a device in the IoT Hub identity registry.</span></span> <span data-ttu-id="64528-282">Tyto dvě metody přijímají **zařízení** instance jako vstup.</span><span class="sxs-lookup"><span data-stu-id="64528-282">These two methods take a **Device** instance as input.</span></span> <span data-ttu-id="64528-283">**Zařízení** třída zahrnuje **ověřování** vlastnost, která vám umožní určit primární a sekundární X.509 kryptografické otisky certifikátu.</span><span class="sxs-lookup"><span data-stu-id="64528-283">The **Device** class includes an **Authentication** property that allows you to specify primary and secondary X.509 certificate thumbprints.</span></span> <span data-ttu-id="64528-284">Kryptografický otisk představuje hodnotu hash SHA-1 (uložené pomocí binární kódování DER) X.509 certifikátu.</span><span class="sxs-lookup"><span data-stu-id="64528-284">The thumbprint represents a SHA-1 hash of the X.509 certificate (stored using binary DER encoding).</span></span> <span data-ttu-id="64528-285">Máte možnost určení primární kryptografický otisk nebo sekundární kryptografický otisk nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="64528-285">You have the option of specifying a primary thumbprint or a secondary thumbprint or both.</span></span> <span data-ttu-id="64528-286">Primární a sekundární kryptografické otisky jsou podporovány pro zpracování scénáře výměnu certifikátů.</span><span class="sxs-lookup"><span data-stu-id="64528-286">Primary and secondary thumbprints are supported to handle certificate rollover scenarios.</span></span>

> [!NOTE]
> <span data-ttu-id="64528-287">IoT Hub nevyžaduje ani uložit celý certifikát X.509 pouze kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="64528-287">IoT Hub does not require or store the entire X.509 certificate, only the thumbprint.</span></span>

<span data-ttu-id="64528-288">Zde je ukázka C\# fragment kódu registrovat zařízení pomocí certifikátu X.509. certifikát:</span><span class="sxs-lookup"><span data-stu-id="64528-288">Here is a sample C\# code snippet to register a device using an X.509 certificate:</span></span>

```csharp
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-certificate-during-run-time-operations"></a><span data-ttu-id="64528-289">Použít certifikát X.509 během spuštění operací</span><span class="sxs-lookup"><span data-stu-id="64528-289">Use an X.509 certificate during run-time operations</span></span>

<span data-ttu-id="64528-290">[Zařízení Azure IoT SDK pro .NET] [ lnk-client-sdk] (verze 1.0.11+) podporuje použití certifikátů X.509.</span><span class="sxs-lookup"><span data-stu-id="64528-290">The [Azure IoT device SDK for .NET][lnk-client-sdk] (version 1.0.11+) supports the use of X.509 certificates.</span></span>

### <a name="c-support"></a><span data-ttu-id="64528-291">C\# podpory</span><span class="sxs-lookup"><span data-stu-id="64528-291">C\# Support</span></span>

<span data-ttu-id="64528-292">Třída **DeviceAuthenticationWithX509Certificate** podporuje vytváření **DeviceClient** instance, používá certifikát X.509.</span><span class="sxs-lookup"><span data-stu-id="64528-292">The class **DeviceAuthenticationWithX509Certificate** supports the creation of **DeviceClient** instances using an X.509 certificate.</span></span> <span data-ttu-id="64528-293">Certifikát X.509 musí být ve formátu PFX (také nazývané PKCS #12), který obsahuje soukromý klíč.</span><span class="sxs-lookup"><span data-stu-id="64528-293">The X.509 certificate must be in the PFX (also called PKCS #12) format that includes the private key.</span></span>

<span data-ttu-id="64528-294">Zde je fragment kódu ukázka:</span><span class="sxs-lookup"><span data-stu-id="64528-294">Here is a sample code snippet:</span></span>

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a><span data-ttu-id="64528-295">Ověřování vlastní zařízení</span><span class="sxs-lookup"><span data-stu-id="64528-295">Custom device authentication</span></span>

<span data-ttu-id="64528-296">Můžete použít Centrum IoT [registru identit] [ lnk-identity-registry] ke konfiguraci zařízení zabezpečovací pověření a přistupovat pomocí ovládacího prvku [tokeny][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="64528-296">You can use the IoT Hub [identity registry][lnk-identity-registry] to configure per-device security credentials and access control using [tokens][lnk-sas-tokens].</span></span> <span data-ttu-id="64528-297">Pokud řešení IoT už má vlastní identitu registru nebo ověřování schématu, zvažte vytvoření *token služby* integrovat do této infrastruktury službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="64528-297">If an IoT solution already has a custom identity registry and/or authentication scheme, consider creating a *token service* to integrate this infrastructure with IoT Hub.</span></span> <span data-ttu-id="64528-298">Tímto způsobem můžete další funkce IoT ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="64528-298">In this way, you can use other IoT features in your solution.</span></span>

<span data-ttu-id="64528-299">Token služby je vlastní Cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="64528-299">A token service is a custom cloud service.</span></span> <span data-ttu-id="64528-300">Používá služby IoT Hub *sdílené zásady přístupu* s **DeviceConnect** oprávnění k vytvoření *obor zařízení* tokeny.</span><span class="sxs-lookup"><span data-stu-id="64528-300">It uses an IoT Hub *shared access policy* with **DeviceConnect** permissions to create *device-scoped* tokens.</span></span> <span data-ttu-id="64528-301">Tyto tokeny povolit zařízení pro připojení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="64528-301">These tokens enable a device to connect to your IoT hub.</span></span>

![Kroky vzoru služby tokenů][img-tokenservice]

<span data-ttu-id="64528-303">Zde jsou hlavní kroky vzoru tokenu služby:</span><span class="sxs-lookup"><span data-stu-id="64528-303">Here are the main steps of the token service pattern:</span></span>

1. <span data-ttu-id="64528-304">Vytvoření služby IoT Hub sdílených zásad přístupu s **DeviceConnect** oprávnění pro službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="64528-304">Create an IoT Hub shared access policy with **DeviceConnect** permissions for your IoT hub.</span></span> <span data-ttu-id="64528-305">Můžete vytvořit v tyto zásady [portál Azure] [ lnk-management-portal] nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="64528-305">You can create this policy in the [Azure portal][lnk-management-portal] or programmatically.</span></span> <span data-ttu-id="64528-306">Službu token tuto zásadu používá k podepisování tokenů, který vytváří.</span><span class="sxs-lookup"><span data-stu-id="64528-306">The token service uses this policy to sign the tokens it creates.</span></span>
1. <span data-ttu-id="64528-307">Pokud zařízení potřebuje přístup k službě IoT hub, vyžádá podepsaný token ze služby tokenu.</span><span class="sxs-lookup"><span data-stu-id="64528-307">When a device needs to access your IoT hub, it requests a signed token from your token service.</span></span> <span data-ttu-id="64528-308">Zařízení můžete ověřit se schématem registru nebo ověření vaše vlastní identity k určení identity zařízení, která služba token používá k vytvoření tohoto tokenu.</span><span class="sxs-lookup"><span data-stu-id="64528-308">The device can authenticate with your custom identity registry/authentication scheme to determine the device identity that the token service uses to create the token.</span></span>
1. <span data-ttu-id="64528-309">Služba tokenu vrátí token.</span><span class="sxs-lookup"><span data-stu-id="64528-309">The token service returns a token.</span></span> <span data-ttu-id="64528-310">Token je vytvořená pomocí `/devices/{deviceId}` jako `resourceURI`, s `deviceId` jako ověřovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-310">The token is created by using `/devices/{deviceId}` as `resourceURI`, with `deviceId` as the device being authenticated.</span></span> <span data-ttu-id="64528-311">Služba tokenu použije k vytvoření tokenu zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="64528-311">The token service uses the shared access policy to construct the token.</span></span>
1. <span data-ttu-id="64528-312">Zařízení na základě tokenu přímo službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="64528-312">The device uses the token directly with the IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="64528-313">Můžete použít třídu .NET [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] nebo třída Java [IotHubServiceSasToken] [ lnk-java-sas] k vytvoření tokenu ve vaší Služba tokenu.</span><span class="sxs-lookup"><span data-stu-id="64528-313">You can use the .NET class [SharedAccessSignatureBuilder][lnk-dotnet-sas] or the Java class [IotHubServiceSasToken][lnk-java-sas] to create a token in your token service.</span></span>

<span data-ttu-id="64528-314">Služba tokenu můžete podle potřeby nastavit vypršení platnosti tokenu.</span><span class="sxs-lookup"><span data-stu-id="64528-314">The token service can set the token expiration as desired.</span></span> <span data-ttu-id="64528-315">Když vyprší platnost tokenu, servery služby IoT hub připojení zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-315">When the token expires, the IoT hub severs the device connection.</span></span> <span data-ttu-id="64528-316">Potom zařízení musíte požádat o nový token od služby tokenů.</span><span class="sxs-lookup"><span data-stu-id="64528-316">Then, the device must request a new token from the token service.</span></span> <span data-ttu-id="64528-317">Čas vypršení platnosti krátké zvyšuje zatížení zařízení a služby tokenů.</span><span class="sxs-lookup"><span data-stu-id="64528-317">A short expiry time increases the load on both the device and the token service.</span></span>

<span data-ttu-id="64528-318">U zařízení pro připojení do vašeho centra, musí stále ho přidáte do registru identit služby IoT Hub – i když zařízení používá token a klíč zařízení pro připojení.</span><span class="sxs-lookup"><span data-stu-id="64528-318">For a device to connect to your hub, you must still add it to the IoT Hub identity registry — even though the device is using a token and not a device key to connect.</span></span> <span data-ttu-id="64528-319">Proto můžete nadále používat řízení přístupu podle zařízení povolením nebo zakázáním identit zařízení ve [registru identit][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="64528-319">Therefore, you can continue to use per-device access control by enabling or disabling device identities in the [identity registry][lnk-identity-registry].</span></span> <span data-ttu-id="64528-320">Tento přístup snižuje rizika tokeny pomocí vypršení platnosti dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="64528-320">This approach mitigates the risks of using tokens with long expiry times.</span></span>

### <a name="comparison-with-a-custom-gateway"></a><span data-ttu-id="64528-321">Porovnání s vlastní bránu</span><span class="sxs-lookup"><span data-stu-id="64528-321">Comparison with a custom gateway</span></span>

<span data-ttu-id="64528-322">Vzor token služby je doporučeným způsobem, jak implementovat vlastní identitu schéma registru nebo ověřování službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="64528-322">The token service pattern is the recommended way to implement a custom identity registry/authentication scheme with IoT Hub.</span></span> <span data-ttu-id="64528-323">Tento vzor je doporučená, protože nadále zpracovávat většina dat řešení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="64528-323">This pattern is recommended because IoT Hub continues to handle most of the solution traffic.</span></span> <span data-ttu-id="64528-324">Ale pokud schéma vlastní ověřování tak vzájemně propojeny s protokolem, možná budete potřebovat *vlastní brány* zpracovat veškerý provoz.</span><span class="sxs-lookup"><span data-stu-id="64528-324">However, if the custom authentication scheme is so intertwined with the protocol, you may require a *custom gateway* to process all the traffic.</span></span> <span data-ttu-id="64528-325">Příkladem takové situaci je pomocí[zabezpečení TLS (Transport Layer) a předsdílených klíčů (PSKs)][lnk-tls-psk].</span><span class="sxs-lookup"><span data-stu-id="64528-325">An example of such a scenario is using[Transport Layer Security (TLS) and pre-shared keys (PSKs)][lnk-tls-psk].</span></span> <span data-ttu-id="64528-326">Další informace najdete v tématu [brány protokolu] [ lnk-protocols] tématu.</span><span class="sxs-lookup"><span data-stu-id="64528-326">For more information, see the [protocol gateway][lnk-protocols] topic.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="64528-327">Témata odkazů:</span><span class="sxs-lookup"><span data-stu-id="64528-327">Reference topics:</span></span>

<span data-ttu-id="64528-328">Následující referenční témata poskytují další informace o řízení přístupu ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="64528-328">The following reference topics provide you with more information about controlling access to your IoT hub.</span></span>

## <a name="iot-hub-permissions"></a><span data-ttu-id="64528-329">IoT Hub oprávnění</span><span class="sxs-lookup"><span data-stu-id="64528-329">IoT Hub permissions</span></span>

<span data-ttu-id="64528-330">Následující tabulka uvádí oprávnění, která můžete použít k řízení přístupu ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="64528-330">The following table lists the permissions you can use to control access to your IoT hub.</span></span>

| <span data-ttu-id="64528-331">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="64528-331">Permission</span></span> | <span data-ttu-id="64528-332">Poznámky</span><span class="sxs-lookup"><span data-stu-id="64528-332">Notes</span></span> |
| --- | --- |
| <span data-ttu-id="64528-333">**RegistryRead**</span><span class="sxs-lookup"><span data-stu-id="64528-333">**RegistryRead**</span></span> |<span data-ttu-id="64528-334">Uděluje přístup do registru identit pro čtení.</span><span class="sxs-lookup"><span data-stu-id="64528-334">Grants read access to the identity registry.</span></span> <span data-ttu-id="64528-335">Další informace najdete v tématu [registru identit][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="64528-335">For more information, see [Identity registry][lnk-identity-registry].</span></span> <br/><span data-ttu-id="64528-336">Toto oprávnění je používán back-end cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="64528-336">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="64528-337">**RegistryReadWrite**</span><span class="sxs-lookup"><span data-stu-id="64528-337">**RegistryReadWrite**</span></span> |<span data-ttu-id="64528-338">Uděluje přístup čtení a zápisu do registru identit.</span><span class="sxs-lookup"><span data-stu-id="64528-338">Grants read and write access to the identity registry.</span></span> <span data-ttu-id="64528-339">Další informace najdete v tématu [registru identit][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="64528-339">For more information, see [Identity registry][lnk-identity-registry].</span></span> <br/><span data-ttu-id="64528-340">Toto oprávnění je používán back-end cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="64528-340">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="64528-341">**ServiceConnect**</span><span class="sxs-lookup"><span data-stu-id="64528-341">**ServiceConnect**</span></span> |<span data-ttu-id="64528-342">Uděluje přístup do cloudu komunikace a sledování koncových bodů služby přístupem.</span><span class="sxs-lookup"><span data-stu-id="64528-342">Grants access to cloud service-facing communication and monitoring endpoints.</span></span> <br/><span data-ttu-id="64528-343">Udělí oprávnění přijímat zprávy typu zařízení cloud, odesílání zpráv typu cloud zařízení a načíst odpovídající potvrzování doručení.</span><span class="sxs-lookup"><span data-stu-id="64528-343">Grants permission to receive device-to-cloud messages, send cloud-to-device messages, and retrieve the corresponding delivery acknowledgments.</span></span> <br/><span data-ttu-id="64528-344">Uděluje oprávnění k načtení potvrzení o doručení pro soubor odešle.</span><span class="sxs-lookup"><span data-stu-id="64528-344">Grants permission to retrieve delivery acknowledgements for file uploads.</span></span> <br/><span data-ttu-id="64528-345">Uděluje oprávnění k přístupu k dvojčata zařízení, které chcete aktualizovat požadované vlastnosti a značky, načíst hlášené vlastnosti a spouštět dotazy.</span><span class="sxs-lookup"><span data-stu-id="64528-345">Grants permission to access device twins to update tags and desired properties, retrieve reported properties, and run queries.</span></span> <br/><span data-ttu-id="64528-346">Toto oprávnění je používán back-end cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="64528-346">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="64528-347">**DeviceConnect**</span><span class="sxs-lookup"><span data-stu-id="64528-347">**DeviceConnect**</span></span> |<span data-ttu-id="64528-348">Uděluje přístup k zařízení přístupem koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="64528-348">Grants access to device-facing endpoints.</span></span> <br/><span data-ttu-id="64528-349">Uděluje oprávnění k odesílání zpráv typu zařízení cloud a příjem zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-349">Grants permission to send device-to-cloud messages and receive cloud-to-device messages.</span></span> <br/><span data-ttu-id="64528-350">Uděluje oprávnění k provedení nahrávání souborů ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-350">Grants permission to perform file upload from a device.</span></span> <br/><span data-ttu-id="64528-351">Uděluje oprávnění k přijímání oznámení vlastnost twin požadovaného zařízení a aktualizaci dvojče zařízení hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="64528-351">Grants permission to receive device twin desired property notifications and update device twin reported properties.</span></span> <br/><span data-ttu-id="64528-352">Uděluje oprávnění k provedení soubor odešle.</span><span class="sxs-lookup"><span data-stu-id="64528-352">Grants permission to perform file uploads.</span></span> <br/><span data-ttu-id="64528-353">Toto oprávnění je používán zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-353">This permission is used by devices.</span></span> |

## <a name="additional-reference-material"></a><span data-ttu-id="64528-354">Odkaz na další materiály</span><span class="sxs-lookup"><span data-stu-id="64528-354">Additional reference material</span></span>

<span data-ttu-id="64528-355">Další témata referenční příručka vývojáře IoT Hub patří:</span><span class="sxs-lookup"><span data-stu-id="64528-355">Other reference topics in the IoT Hub developer guide include:</span></span>

* <span data-ttu-id="64528-356">[Koncové body centra IoT] [ lnk-endpoints] popisuje různé koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.</span><span class="sxs-lookup"><span data-stu-id="64528-356">[IoT Hub endpoints][lnk-endpoints] describes the various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="64528-357">[Omezování a kvóty] [ lnk-quotas] popisuje kvóty a omezení chování, které se vztahují ke službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="64528-357">[Throttling and quotas][lnk-quotas] describes the quotas and throttling behaviors that apply to the IoT Hub service.</span></span>
* <span data-ttu-id="64528-358">[Azure IoT zařízení a služby sady SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.</span><span class="sxs-lookup"><span data-stu-id="64528-358">[Azure IoT device and service SDKs][lnk-sdks] lists the various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="64528-359">[IoT Hub dotazovací jazyk] [ lnk-query] popisuje dotazovací jazyk, můžete použít k načtení informací ze služby IoT Hub o úlohách a dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="64528-359">[IoT Hub query language][lnk-query] describes the query language you can use to retrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="64528-360">[Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT.</span><span class="sxs-lookup"><span data-stu-id="64528-360">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for the MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="64528-361">Další kroky</span><span class="sxs-lookup"><span data-stu-id="64528-361">Next steps</span></span>

<span data-ttu-id="64528-362">Nyní jste se naučili řízení přístupu služby IoT Hub, může zajímat v následujících tématech Příručka vývojáře IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="64528-362">Now you have learned how to control access IoT Hub, you may be interested in the following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="64528-363">[Pomocí dvojčata zařízení synchronizovat stavu a konfigurace][lnk-devguide-device-twins]</span><span class="sxs-lookup"><span data-stu-id="64528-363">[Use device twins to synchronize state and configurations][lnk-devguide-device-twins]</span></span>
* <span data-ttu-id="64528-364">[Volání metody přímé na zařízení][lnk-devguide-directmethods]</span><span class="sxs-lookup"><span data-stu-id="64528-364">[Invoke a direct method on a device][lnk-devguide-directmethods]</span></span>
* <span data-ttu-id="64528-365">[Plánování úloh na několika zařízeních][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="64528-365">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="64528-366">Pokud chcete vyzkoušet některé konceptů popsaných v tomto článku, může zajímat v následujících kurzech IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="64528-366">If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorials:</span></span>

* <span data-ttu-id="64528-367">[Začínáme s Azure IoT Hub][lnk-getstarted-tutorial]</span><span class="sxs-lookup"><span data-stu-id="64528-367">[Get started with Azure IoT Hub][lnk-getstarted-tutorial]</span></span>
* <span data-ttu-id="64528-368">[Odesílání zpráv typu cloud zařízení s centrem IoT][lnk-c2d-tutorial]</span><span class="sxs-lookup"><span data-stu-id="64528-368">[How to send cloud-to-device messages with IoT Hub][lnk-c2d-tutorial]</span></span>
* <span data-ttu-id="64528-369">[Postupy zpracování zpráv typu zařízení cloud IoT Hub][lnk-d2c-tutorial]</span><span class="sxs-lookup"><span data-stu-id="64528-369">[How to process IoT Hub device-to-cloud messages][lnk-d2c-tutorial]</span></span>

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth._iot_hub_service_sas_token
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
