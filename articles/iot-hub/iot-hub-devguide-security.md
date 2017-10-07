---
title: "aaaUnderstand zabezpečení Azure IoT Hub | Microsoft Docs"
description: "Průvodce - vývojáře přístupu toocontrol tooIoT rozbočovače pro aplikace pro zařízení a back-end aplikace. Obsahuje informace o tokeny zabezpečení a podporu pro certifikáty X.509."
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
ms.openlocfilehash: 717726328a6bb5c5c334a123d0abfed711b2c3b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="control-access-tooiot-hub"></a><span data-ttu-id="fea6c-104">Řízení přístupu tooIoT rozbočovače</span><span class="sxs-lookup"><span data-stu-id="fea6c-104">Control access tooIoT Hub</span></span>

<span data-ttu-id="fea6c-105">Tento článek popisuje možnosti hello pro zabezpečení služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fea6c-105">This article describes hello options for securing your IoT hub.</span></span> <span data-ttu-id="fea6c-106">IoT Hub používá *oprávnění* toogrant koncový bod centra IoT přístup tooeach.</span><span class="sxs-lookup"><span data-stu-id="fea6c-106">IoT Hub uses *permissions* toogrant access tooeach IoT hub endpoint.</span></span> <span data-ttu-id="fea6c-107">Oprávnění omezit hello přístup tooan IoT hub závislosti na funkcích.</span><span class="sxs-lookup"><span data-stu-id="fea6c-107">Permissions limit hello access tooan IoT hub based on functionality.</span></span>

<span data-ttu-id="fea6c-108">Tento článek popisuje:</span><span class="sxs-lookup"><span data-stu-id="fea6c-108">This article describes:</span></span>

* <span data-ttu-id="fea6c-109">Hello jiná oprávnění, můžete udělit nebo back-end aplikačním tooaccess tooa zařízení služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fea6c-109">hello different permissions that you can grant tooa device or back-end app tooaccess your IoT hub.</span></span>
* <span data-ttu-id="fea6c-110">Hello proces a hello tokeny ověřování používá tooverify oprávnění.</span><span class="sxs-lookup"><span data-stu-id="fea6c-110">hello authentication process and hello tokens it uses tooverify permissions.</span></span>
* <span data-ttu-id="fea6c-111">Jak tooscope pověření toolimit přístup k toospecific prostředkům.</span><span class="sxs-lookup"><span data-stu-id="fea6c-111">How tooscope credentials toolimit access toospecific resources.</span></span>
* <span data-ttu-id="fea6c-112">Podpora služby IoT Hub pro certifikáty X.509.</span><span class="sxs-lookup"><span data-stu-id="fea6c-112">IoT Hub support for X.509 certificates.</span></span>
* <span data-ttu-id="fea6c-113">Mechanismy ověřování vlastní zařízení, které používají existující registrech identity zařízení nebo schémat ověřování.</span><span class="sxs-lookup"><span data-stu-id="fea6c-113">Custom device authentication mechanisms that use existing device identity registries or authentication schemes.</span></span>

### <a name="when-toouse"></a><span data-ttu-id="fea6c-114">Když toouse</span><span class="sxs-lookup"><span data-stu-id="fea6c-114">When toouse</span></span>

<span data-ttu-id="fea6c-115">Musí mít příslušná oprávnění tooaccess žádné koncové body centra IoT hello.</span><span class="sxs-lookup"><span data-stu-id="fea6c-115">You must have appropriate permissions tooaccess any of hello IoT Hub endpoints.</span></span> <span data-ttu-id="fea6c-116">Zařízení musí obsahovat třeba token obsahující zabezpečovací pověření společně s každou zprávu, že odešle tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="fea6c-116">For example, a device must include a token containing security credentials along with every message it sends tooIoT Hub.</span></span>

## <a name="access-control-and-permissions"></a><span data-ttu-id="fea6c-117">Řízení přístupu a oprávnění</span><span class="sxs-lookup"><span data-stu-id="fea6c-117">Access control and permissions</span></span>

<span data-ttu-id="fea6c-118">Můžete udělit [oprávnění](#iot-hub-permissions) v hello následující způsoby:</span><span class="sxs-lookup"><span data-stu-id="fea6c-118">You can grant [permissions](#iot-hub-permissions) in hello following ways:</span></span>

* <span data-ttu-id="fea6c-119">**IoT hub úrovni sdílených zásad přístupu**.</span><span class="sxs-lookup"><span data-stu-id="fea6c-119">**IoT hub-level shared access policies**.</span></span> <span data-ttu-id="fea6c-120">Zásady sdíleného přístupu můžete udělit libovolnou kombinaci [oprávnění](#iot-hub-permissions).</span><span class="sxs-lookup"><span data-stu-id="fea6c-120">Shared access policies can grant any combination of [permissions](#iot-hub-permissions).</span></span> <span data-ttu-id="fea6c-121">Zásady můžete definovat v hello [portál Azure][lnk-management-portal], nebo programově pomocí hello [zprostředkovatele prostředků služby IoT Hub rozhraní REST API][lnk-resource-provider-apis].</span><span class="sxs-lookup"><span data-stu-id="fea6c-121">You can define policies in hello [Azure portal][lnk-management-portal], or programmatically by using hello [IoT Hub resource provider REST APIs][lnk-resource-provider-apis].</span></span> <span data-ttu-id="fea6c-122">Nově vytvořený IoT hub má hello následující výchozí zásady:</span><span class="sxs-lookup"><span data-stu-id="fea6c-122">A newly created IoT hub has hello following default policies:</span></span>

  * <span data-ttu-id="fea6c-123">**iothubowner**: zásada se všechna oprávnění.</span><span class="sxs-lookup"><span data-stu-id="fea6c-123">**iothubowner**: Policy with all permissions.</span></span>
  * <span data-ttu-id="fea6c-124">**Služba**: zásada se **ServiceConnect** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="fea6c-124">**service**: Policy with **ServiceConnect** permission.</span></span>
  * <span data-ttu-id="fea6c-125">**zařízení**: zásada se **DeviceConnect** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="fea6c-125">**device**: Policy with **DeviceConnect** permission.</span></span>
  * <span data-ttu-id="fea6c-126">**registryRead**: zásada se **RegistryRead** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="fea6c-126">**registryRead**: Policy with **RegistryRead** permission.</span></span>
  * <span data-ttu-id="fea6c-127">**registryReadWrite**: zásada se **RegistryRead** a RegistryWrite oprávnění.</span><span class="sxs-lookup"><span data-stu-id="fea6c-127">**registryReadWrite**: Policy with **RegistryRead** and RegistryWrite permissions.</span></span>
  * <span data-ttu-id="fea6c-128">**Podle zařízení zabezpečovací pověření**.</span><span class="sxs-lookup"><span data-stu-id="fea6c-128">**Per-device security credentials**.</span></span> <span data-ttu-id="fea6c-129">Každé centrum IoT obsahuje [registru identit][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="fea6c-129">Each IoT Hub contains an [identity registry][lnk-identity-registry].</span></span> <span data-ttu-id="fea6c-130">Pro každé zařízení v registru této identity můžete nakonfigurovat zabezpečovací pověření, která udělují **DeviceConnect** oprávnění oboru toohello koncové body odpovídající zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-130">For each device in this identity registry, you can configure security credentials that grant **DeviceConnect** permissions scoped toohello corresponding device endpoints.</span></span>

<span data-ttu-id="fea6c-131">Například v typického řešení IoT:</span><span class="sxs-lookup"><span data-stu-id="fea6c-131">For example, in a typical IoT solution:</span></span>

* <span data-ttu-id="fea6c-132">komponenty správy zařízení Hello používá hello *registryReadWrite* zásad.</span><span class="sxs-lookup"><span data-stu-id="fea6c-132">hello device management component uses hello *registryReadWrite* policy.</span></span>
* <span data-ttu-id="fea6c-133">součástí procesoru událostí Hello používá hello *služby* zásad.</span><span class="sxs-lookup"><span data-stu-id="fea6c-133">hello event processor component uses hello *service* policy.</span></span>
* <span data-ttu-id="fea6c-134">Hello spuštění zařízení obchodní logiky součást používá hello *služby* zásad.</span><span class="sxs-lookup"><span data-stu-id="fea6c-134">hello run-time device business logic component uses hello *service* policy.</span></span>
* <span data-ttu-id="fea6c-135">Jednotlivých zařízení se připojují přes přihlašovací údaje uložené v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="fea6c-135">Individual devices connect using credentials stored in hello IoT hub's identity registry.</span></span>

> [!NOTE]
> <span data-ttu-id="fea6c-136">V tématu [oprávnění](#iot-hub-permissions) podrobné informace.</span><span class="sxs-lookup"><span data-stu-id="fea6c-136">See [permissions](#iot-hub-permissions) for detailed information.</span></span>

## <a name="authentication"></a><span data-ttu-id="fea6c-137">Authentication</span><span class="sxs-lookup"><span data-stu-id="fea6c-137">Authentication</span></span>

<span data-ttu-id="fea6c-138">Azure IoT Hub uděluje přístup tooendpoints kontrolou token proti hello sdílené zásady přístupu a identit registru zabezpečovací pověření.</span><span class="sxs-lookup"><span data-stu-id="fea6c-138">Azure IoT Hub grants access tooendpoints by verifying a token against hello shared access policies and identity registry security credentials.</span></span>

<span data-ttu-id="fea6c-139">Zabezpečovací přihlašovací údaje, jako jsou symetrického klíče, se nikdy odešlou přes přenosu hello.</span><span class="sxs-lookup"><span data-stu-id="fea6c-139">Security credentials, such as symmetric keys, are never sent over hello wire.</span></span>

> [!NOTE]
> <span data-ttu-id="fea6c-140">Hello poskytovatel prostředků Azure IoT Hub je zabezpečená vašeho předplatného Azure, jako jsou všechny zprostředkovatele v hello [Azure Resource Manager][lnk-azure-resource-manager].</span><span class="sxs-lookup"><span data-stu-id="fea6c-140">hello Azure IoT Hub resource provider is secured through your Azure subscription, as are all providers in hello [Azure Resource Manager][lnk-azure-resource-manager].</span></span>

<span data-ttu-id="fea6c-141">Další informace o tom, najdete v části tooconstruct a použití tokenů zabezpečení [tokeny zabezpečení služby IoT Hub][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="fea6c-141">For more information about how tooconstruct and use security tokens, see [IoT Hub security tokens][lnk-sas-tokens].</span></span>

### <a name="protocol-specifics"></a><span data-ttu-id="fea6c-142">Protokol podrobností</span><span class="sxs-lookup"><span data-stu-id="fea6c-142">Protocol specifics</span></span>

<span data-ttu-id="fea6c-143">Každý podporovaný protokol, například MQTT, AMQP a HTTP, je určena k přenosu tokeny různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="fea6c-143">Each supported protocol, such as MQTT, AMQP, and HTTP, transports tokens in different ways.</span></span>

<span data-ttu-id="fea6c-144">Při použití MQTT, hello připojení paketu má hello deviceId jako hello ClientId, {iothubhostname} / {deviceId} v hello pole pro uživatelské jméno a do pole heslo hello tokenu SAS.</span><span class="sxs-lookup"><span data-stu-id="fea6c-144">When using MQTT, hello CONNECT packet has hello deviceId as hello ClientId, {iothubhostname}/{deviceId} in hello Username field, and a SAS token in hello Password field.</span></span> <span data-ttu-id="fea6c-145">{iothubhostname} by měl být hello úplné CName hello IoT hub (například contoso.azure-devices.net).</span><span class="sxs-lookup"><span data-stu-id="fea6c-145">{iothubhostname} should be hello full CName of hello IoT hub (for example, contoso.azure-devices.net).</span></span>

<span data-ttu-id="fea6c-146">Při použití [AMQP][lnk-amqp], podporuje IoT Hub [SASL prostý] [ lnk-sasl-plain] a [AMQP deklarace identity – zabezpečení na základě-] [ lnk-cbs].</span><span class="sxs-lookup"><span data-stu-id="fea6c-146">When using [AMQP][lnk-amqp], IoT Hub supports [SASL PLAIN][lnk-sasl-plain] and [AMQP Claims-Based-Security][lnk-cbs].</span></span>

<span data-ttu-id="fea6c-147">Pokud používáte AMQP deklarace identity – zabezpečení na základě-, určuje hello standardní jak tootransmit tyto tokeny.</span><span class="sxs-lookup"><span data-stu-id="fea6c-147">If you use AMQP claims-based-security, hello standard specifies how tootransmit these tokens.</span></span>

<span data-ttu-id="fea6c-148">SASL prostý text hello **uživatelské jméno** může být:</span><span class="sxs-lookup"><span data-stu-id="fea6c-148">For SASL PLAIN, hello **username** can be:</span></span>

* <span data-ttu-id="fea6c-149">`{policyName}@sas.root.{iothubName}`Pokud se používá tokeny úrovni centra IoT.</span><span class="sxs-lookup"><span data-stu-id="fea6c-149">`{policyName}@sas.root.{iothubName}` if using IoT hub-level tokens.</span></span>
* <span data-ttu-id="fea6c-150">`{deviceId}@sas.{iothubname}`Pokud používáte zařízení obor tokeny.</span><span class="sxs-lookup"><span data-stu-id="fea6c-150">`{deviceId}@sas.{iothubname}` if using device-scoped tokens.</span></span>

<span data-ttu-id="fea6c-151">V obou případech pole hesla hello obsahuje hello token, jak je popsáno v [tokeny zabezpečení služby IoT Hub][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="fea6c-151">In both cases, hello password field contains hello token, as described in [IoT Hub security tokens][lnk-sas-tokens].</span></span>

<span data-ttu-id="fea6c-152">HTTP implementuje ověřování zahrnutím platný token v hello **autorizace** hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="fea6c-152">HTTP implements authentication by including a valid token in hello **Authorization** request header.</span></span>

#### <a name="example"></a><span data-ttu-id="fea6c-153">Příklad</span><span class="sxs-lookup"><span data-stu-id="fea6c-153">Example</span></span>

<span data-ttu-id="fea6c-154">Uživatelské jméno (DeviceId je malá a velká písmena):`iothubname.azure-devices.net/DeviceId`</span><span class="sxs-lookup"><span data-stu-id="fea6c-154">Username (DeviceId is case-sensitive): `iothubname.azure-devices.net/DeviceId`</span></span>

<span data-ttu-id="fea6c-155">Heslo (Generovat SAS token s hello [explorer zařízení] [ lnk-device-explorer] nástroj):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`</span><span class="sxs-lookup"><span data-stu-id="fea6c-155">Password (Generate SAS token with hello [device explorer][lnk-device-explorer] tool): `SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`</span></span>

> [!NOTE]
> <span data-ttu-id="fea6c-156">Hello [SDK služby Azure IoT] [ lnk-sdks] automaticky generovat tokeny při připojování toohello služby.</span><span class="sxs-lookup"><span data-stu-id="fea6c-156">hello [Azure IoT SDKs][lnk-sdks] automatically generate tokens when connecting toohello service.</span></span> <span data-ttu-id="fea6c-157">V některých případech nepodporují hello SDK služby Azure IoT všechny protokoly hello nebo všechny metody ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="fea6c-157">In some cases, hello Azure IoT SDKs do not support all hello protocols or all hello authentication methods.</span></span>

### <a name="special-considerations-for-sasl-plain"></a><span data-ttu-id="fea6c-158">Zvláštní upozornění pro SASL prostý</span><span class="sxs-lookup"><span data-stu-id="fea6c-158">Special considerations for SASL PLAIN</span></span>

<span data-ttu-id="fea6c-159">Při použití SASL prostý s AMQP, klient připojení tooan IoT hub můžete použít jeden token pro každé připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="fea6c-159">When using SASL PLAIN with AMQP, a client connecting tooan IoT hub can use a single token for each TCP connection.</span></span> <span data-ttu-id="fea6c-160">Když vyprší platnost tokenu hello, hello připojení TCP odpojí od hello služby a aktivuje o obnovení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-160">When hello token expires, hello TCP connection disconnects from hello service and triggers a reconnect.</span></span> <span data-ttu-id="fea6c-161">Toto chování, když není problematické pro back-end aplikačním je poškození aplikace zařízení pro hello následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="fea6c-161">This behavior, while not problematic for a back-end app, is damaging for a device app for hello following reasons:</span></span>

* <span data-ttu-id="fea6c-162">Brány obvykle jménem mnoho zařízení připojit.</span><span class="sxs-lookup"><span data-stu-id="fea6c-162">Gateways usually connect on behalf of many devices.</span></span> <span data-ttu-id="fea6c-163">Pokud používáte SASL prostý, mají toocreate odlišné připojení TCP pro každé zařízení připojení tooan IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fea6c-163">When using SASL PLAIN, they have toocreate a distinct TCP connection for each device connecting tooan IoT hub.</span></span> <span data-ttu-id="fea6c-164">Tento scénář také výrazně zvyšuje hello spotřeba energie a síťových prostředků a hello latence každé připojení zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-164">This scenario considerably increases hello consumption of power and networking resources, and increases hello latency of each device connection.</span></span>
* <span data-ttu-id="fea6c-165">Zařízení s omezenými zdroji jsou negativně ovlivněn pomocí hello vyšší prostředky tooreconnect po každém vypršení platnosti tokenu.</span><span class="sxs-lookup"><span data-stu-id="fea6c-165">Resource-constrained devices are adversely affected by hello increased use of resources tooreconnect after each token expiration.</span></span>

## <a name="scope-iot-hub-level-credentials"></a><span data-ttu-id="fea6c-166">Přihlašovací údaje úrovni centra IoT oboru</span><span class="sxs-lookup"><span data-stu-id="fea6c-166">Scope IoT hub-level credentials</span></span>

<span data-ttu-id="fea6c-167">Můžete určit obor zásad zabezpečení na úrovni centra IoT tak, že vytvoříte tokeny pomocí identifikátoru URI prostředku s omezeným přístupem.</span><span class="sxs-lookup"><span data-stu-id="fea6c-167">You can scope IoT hub-level security policies by creating tokens with a restricted resource URI.</span></span> <span data-ttu-id="fea6c-168">Například hello koncový bod toosend zařízení cloud zprávy ze zařízení je **/devices/ {deviceId} / zprávy/události**.</span><span class="sxs-lookup"><span data-stu-id="fea6c-168">For example, hello endpoint toosend device-to-cloud messages from a device is **/devices/{deviceId}/messages/events**.</span></span> <span data-ttu-id="fea6c-169">Můžete taky zásady sdíleného přístupu úrovně rozbočovače IoT s **DeviceConnect** oprávnění toosign token je jejichž resourceURI **/devices/ {deviceId}**.</span><span class="sxs-lookup"><span data-stu-id="fea6c-169">You can also use an IoT hub-level shared access policy with **DeviceConnect** permissions toosign a token whose resourceURI is **/devices/{deviceId}**.</span></span> <span data-ttu-id="fea6c-170">Tento postup vytvoří token, který je pouze použitelné toosend zprávy jménem zařízení **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="fea6c-170">This approach creates a token that is only usable toosend messages on behalf of device **deviceId**.</span></span>

<span data-ttu-id="fea6c-171">Tento mechanismus je podobné toohello [zásad vydavatele Event Hubs][lnk-event-hubs-publisher-policy]a umožní vám tooimplement vlastní metody ověřování.</span><span class="sxs-lookup"><span data-stu-id="fea6c-171">This mechanism is similar toohello [Event Hubs publisher policy][lnk-event-hubs-publisher-policy], and enables you tooimplement custom authentication methods.</span></span>

## <a name="security-tokens"></a><span data-ttu-id="fea6c-172">Tokeny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="fea6c-172">Security tokens</span></span>

<span data-ttu-id="fea6c-173">Zabezpečení služby IoT Hub používá tokeny tooauthenticate zařízení a služeb tooavoid odesílání klíčů na hello přenosu.</span><span class="sxs-lookup"><span data-stu-id="fea6c-173">IoT Hub uses security tokens tooauthenticate devices and services tooavoid sending keys on hello wire.</span></span> <span data-ttu-id="fea6c-174">Kromě toho mají omezenou dobu platnosti a obor tokeny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-174">Additionally, security tokens are limited in time validity and scope.</span></span> <span data-ttu-id="fea6c-175">[Sady SDK služby Azure IoT] [ lnk-sdks] automaticky generovat tokeny bez nutnosti žádnou zvláštní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="fea6c-175">[Azure IoT SDKs][lnk-sdks] automatically generate tokens without requiring any special configuration.</span></span> <span data-ttu-id="fea6c-176">Některé scénáře vyžadují toogenerate a používat přímo tokeny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-176">Some scenarios do require you toogenerate and use security tokens directly.</span></span> <span data-ttu-id="fea6c-177">Mezi tyto scénáře patří:</span><span class="sxs-lookup"><span data-stu-id="fea6c-177">Such scenarios include:</span></span>

* <span data-ttu-id="fea6c-178">Hello přímého použití ploch hello MQTT, AMQP a HTTP.</span><span class="sxs-lookup"><span data-stu-id="fea6c-178">hello direct use of hello MQTT, AMQP, or HTTP surfaces.</span></span>
* <span data-ttu-id="fea6c-179">Hello implementace vzoru hello služba tokenu, jak je popsáno v [ověřování zařízení vlastní][lnk-custom-auth].</span><span class="sxs-lookup"><span data-stu-id="fea6c-179">hello implementation of hello token service pattern, as explained in [Custom device authentication][lnk-custom-auth].</span></span>

<span data-ttu-id="fea6c-180">Centrum IoT také umožňuje zařízení tooauthenticate službou IoT Hub pomocí [certifikáty X.509][lnk-x509].</span><span class="sxs-lookup"><span data-stu-id="fea6c-180">IoT Hub also allows devices tooauthenticate with IoT Hub using [X.509 certificates][lnk-x509].</span></span>

### <a name="security-token-structure"></a><span data-ttu-id="fea6c-181">Struktura tokenu zabezpečení</span><span class="sxs-lookup"><span data-stu-id="fea6c-181">Security token structure</span></span>

<span data-ttu-id="fea6c-182">Použijte zabezpečení tokeny toogrant ohraničenou čas přístup toodevices a služby toospecific funkci v IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="fea6c-182">You use security tokens toogrant time-bounded access toodevices and services toospecific functionality in IoT Hub.</span></span> <span data-ttu-id="fea6c-183">tooget autorizace tooconnect tooIoT rozbočovače, zařízení a služeb musí odesílat tokeny zabezpečení, které jsou podepsané sdíleného přístupu nebo symetrického klíče.</span><span class="sxs-lookup"><span data-stu-id="fea6c-183">tooget authorization tooconnect tooIoT Hub, devices and services must send security tokens signed with either a shared access or symmetric key.</span></span> <span data-ttu-id="fea6c-184">Tyto klíče jsou uloženy s identitu zařízení v registru identit hello.</span><span class="sxs-lookup"><span data-stu-id="fea6c-184">These keys are stored with a device identity in hello identity registry.</span></span>

<span data-ttu-id="fea6c-185">Token podepsán sdílený přístupový klíč uděluje přístup tooall hello funkcí spojených s oprávnění zásad hello sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="fea6c-185">A token signed with a shared access key grants access tooall hello functionality associated with hello shared access policy permissions.</span></span> <span data-ttu-id="fea6c-186">Token podepsaný pomocí identity zařízení symetrického klíče pouze uděluje hello **DeviceConnect** oprávnění pro hello přidružené identitu zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-186">A token signed with a device identity's symmetric key only grants hello **DeviceConnect** permission for hello associated device identity.</span></span>

<span data-ttu-id="fea6c-187">token zabezpečení Hello má hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="fea6c-187">hello security token has hello following format:</span></span>

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

<span data-ttu-id="fea6c-188">Zde jsou hello očekávaných hodnot:</span><span class="sxs-lookup"><span data-stu-id="fea6c-188">Here are hello expected values:</span></span>

| <span data-ttu-id="fea6c-189">Hodnota</span><span class="sxs-lookup"><span data-stu-id="fea6c-189">Value</span></span> | <span data-ttu-id="fea6c-190">Popis</span><span class="sxs-lookup"><span data-stu-id="fea6c-190">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fea6c-191">{podpis}</span><span class="sxs-lookup"><span data-stu-id="fea6c-191">{signature}</span></span> |<span data-ttu-id="fea6c-192">Řetězec podpisu HMAC SHA256 formuláře hello: `{URL-encoded-resourceURI} + "\n" + expiry`.</span><span class="sxs-lookup"><span data-stu-id="fea6c-192">An HMAC-SHA256 signature string of hello form: `{URL-encoded-resourceURI} + "\n" + expiry`.</span></span> <span data-ttu-id="fea6c-193">**Důležité**: klíč hello je dekódovat z formátu base64 a použít jako výpočetní klíče tooperform hello HMAC SHA256.</span><span class="sxs-lookup"><span data-stu-id="fea6c-193">**Important**: hello key is decoded from base64 and used as key tooperform hello HMAC-SHA256 computation.</span></span> |
| <span data-ttu-id="fea6c-194">{resourceURI}</span><span class="sxs-lookup"><span data-stu-id="fea6c-194">{resourceURI}</span></span> |<span data-ttu-id="fea6c-195">Předpony identifikátoru URI (podle segmentu) hello koncových bodů, které lze přistupovat pomocí tohoto tokenu, počínaje název hostitele centra IoT hello (žádné protocol).</span><span class="sxs-lookup"><span data-stu-id="fea6c-195">URI prefix (by segment) of hello endpoints that can be accessed with this token, starting with host name of hello IoT hub (no protocol).</span></span> <span data-ttu-id="fea6c-196">Například `myHub.azure-devices.net/devices/device1`.</span><span class="sxs-lookup"><span data-stu-id="fea6c-196">For example, `myHub.azure-devices.net/devices/device1`</span></span> |
| <span data-ttu-id="fea6c-197">{vypršení platnosti}</span><span class="sxs-lookup"><span data-stu-id="fea6c-197">{expiry}</span></span> |<span data-ttu-id="fea6c-198">Řetězce UTF8 pro počet sekund od hello epoch 00:00:00 UTC na 1. ledna pod hodnotou 1970.</span><span class="sxs-lookup"><span data-stu-id="fea6c-198">UTF8 strings for number of seconds since hello epoch 00:00:00 UTC on 1 January 1970.</span></span> |
| <span data-ttu-id="fea6c-199">{Adresu URL-kódovaný resourceURI}</span><span class="sxs-lookup"><span data-stu-id="fea6c-199">{URL-encoded-resourceURI}</span></span> |<span data-ttu-id="fea6c-200">Nižší případ kódování URL prostředku malá hello identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="fea6c-200">Lower case URL-encoding of hello lower case resource URI</span></span> |
| <span data-ttu-id="fea6c-201">{policyName}</span><span class="sxs-lookup"><span data-stu-id="fea6c-201">{policyName}</span></span> |<span data-ttu-id="fea6c-202">Název Hello hello sdílené toowhich zásady přístupu, který odkazuje tento token.</span><span class="sxs-lookup"><span data-stu-id="fea6c-202">hello name of hello shared access policy toowhich this token refers.</span></span> <span data-ttu-id="fea6c-203">Chybějící Pokud hello token odkazuje toodevice registru přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="fea6c-203">Absent if hello token refers toodevice-registry credentials.</span></span> |

<span data-ttu-id="fea6c-204">**Poznámka: na předponě**: předpony identifikátoru URI hello se počítá podle segmentu a ne serverem znak.</span><span class="sxs-lookup"><span data-stu-id="fea6c-204">**Note on prefix**: hello URI prefix is computed by segment and not by character.</span></span> <span data-ttu-id="fea6c-205">Například `/a/b` předpona pro `/a/b/c` , ale ne pro `/a/bc`.</span><span class="sxs-lookup"><span data-stu-id="fea6c-205">For example `/a/b` is a prefix for `/a/b/c` but not for `/a/bc`.</span></span>

<span data-ttu-id="fea6c-206">Hello následující fragment kódu Node.js ukazuje funkci s názvem **generateSasToken** , výpočtů hello tokenu z hello vstupy `resourceUri, signingKey, policyName, expiresInMins`.</span><span class="sxs-lookup"><span data-stu-id="fea6c-206">hello following Node.js snippet shows a function called **generateSasToken** that computes hello token from hello inputs `resourceUri, signingKey, policyName, expiresInMins`.</span></span> <span data-ttu-id="fea6c-207">Další části Hello podrobnosti, jak tooinitialize hello různé vstupy pro různé token hello případy použití.</span><span class="sxs-lookup"><span data-stu-id="fea6c-207">hello next sections detail how tooinitialize hello different inputs for hello different token use cases.</span></span>

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

<span data-ttu-id="fea6c-208">Ke srovnání hello ekvivalentní toogenerate kód Python, je token zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="fea6c-208">As a comparison, hello equivalent Python code toogenerate a security token is:</span></span>

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
> <span data-ttu-id="fea6c-209">Vzhledem k tomu, že hello doba platnosti tokenu hello se ověří na počítačích IoT Hub, musí být vypnuté hello hello hodiny hello počítače, který generuje hello token minimální.</span><span class="sxs-lookup"><span data-stu-id="fea6c-209">Since hello time validity of hello token is validated on IoT Hub machines, hello drift on hello clock of hello machine that generates hello token must be minimal.</span></span>

### <a name="use-sas-tokens-in-a-device-app"></a><span data-ttu-id="fea6c-210">Použití tokeny SAS v aplikaci pro zařízení</span><span class="sxs-lookup"><span data-stu-id="fea6c-210">Use SAS tokens in a device app</span></span>

<span data-ttu-id="fea6c-211">Existují dva způsoby tooobtain **DeviceConnect** oprávnění službou IoT Hub s tokeny zabezpečení: pomocí [zařízení symetrického klíče z registru identit hello](#use-a-symmetric-key-in-the-identity-registry), nebo použijte [sdílený přístupový klíč](#use-a-shared-access-policy).</span><span class="sxs-lookup"><span data-stu-id="fea6c-211">There are two ways tooobtain **DeviceConnect** permissions with IoT Hub with security tokens: use a [symmetric device key from hello identity registry](#use-a-symmetric-key-in-the-identity-registry), or use a [shared access key](#use-a-shared-access-policy).</span></span>

<span data-ttu-id="fea6c-212">Mějte na paměti, že všechny funkce, které jsou přístupné ze zařízení je zveřejněný prostřednictvím návrhu na koncové body s předponou `/devices/{deviceId}`.</span><span class="sxs-lookup"><span data-stu-id="fea6c-212">Remember that all functionality accessible from devices is exposed by design on endpoints with prefix `/devices/{deviceId}`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fea6c-213">Hello jediným způsobem, že IoT Hub ověřuje určité zařízení používá hello zařízení identity symetrického klíče.</span><span class="sxs-lookup"><span data-stu-id="fea6c-213">hello only way that IoT Hub authenticates a specific device is using hello device identity symmetric key.</span></span> <span data-ttu-id="fea6c-214">V případech, když se zásada sdíleného přístupu použité tooaccess funkce zařízení, musí řešení hello zvažte hello součástí vydání tokenu zabezpečení hello důvěryhodné komponentu.</span><span class="sxs-lookup"><span data-stu-id="fea6c-214">In cases when a shared access policy is used tooaccess device functionality, hello solution must consider hello component issuing hello security token as a trusted subcomponent.</span></span>

<span data-ttu-id="fea6c-215">Hello zařízení přístupem koncových bodů jsou (bez ohledu na protokol hello):</span><span class="sxs-lookup"><span data-stu-id="fea6c-215">hello device-facing endpoints are (irrespective of hello protocol):</span></span>

| <span data-ttu-id="fea6c-216">Koncový bod</span><span class="sxs-lookup"><span data-stu-id="fea6c-216">Endpoint</span></span> | <span data-ttu-id="fea6c-217">Funkce</span><span class="sxs-lookup"><span data-stu-id="fea6c-217">Functionality</span></span> |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |<span data-ttu-id="fea6c-218">Odesílání zpráv typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="fea6c-218">Send device-to-cloud messages.</span></span> |
| `{iot hub host name}/devices/{deviceId}/devicebound` |<span data-ttu-id="fea6c-219">Příjem zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-219">Receive cloud-to-device messages.</span></span> |

### <a name="use-a-symmetric-key-in-hello-identity-registry"></a><span data-ttu-id="fea6c-220">Použít symetrického klíče v registru identit hello</span><span class="sxs-lookup"><span data-stu-id="fea6c-220">Use a symmetric key in hello identity registry</span></span>

<span data-ttu-id="fea6c-221">Pokud používáte identitu zařízení symetrického klíče toogenerate token, hello policyName (`skn`) element hello tokenu je vynechán.</span><span class="sxs-lookup"><span data-stu-id="fea6c-221">When using a device identity's symmetric key toogenerate a token, hello policyName (`skn`) element of hello token is omitted.</span></span>

<span data-ttu-id="fea6c-222">Například token vytvořen tooaccess všechny funkce zařízení by měl mít hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="fea6c-222">For example, a token created tooaccess all device functionality should have hello following parameters:</span></span>

* <span data-ttu-id="fea6c-223">identifikátor URI prostředku: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span><span class="sxs-lookup"><span data-stu-id="fea6c-223">resource URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span></span>
* <span data-ttu-id="fea6c-224">podpisový klíč: žádné symetrický klíč pro hello `{device id}` identitu,</span><span class="sxs-lookup"><span data-stu-id="fea6c-224">signing key: any symmetric key for hello `{device id}` identity,</span></span>
* <span data-ttu-id="fea6c-225">žádný název zásady</span><span class="sxs-lookup"><span data-stu-id="fea6c-225">no policy name,</span></span>
* <span data-ttu-id="fea6c-226">kdykoli vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="fea6c-226">any expiration time.</span></span>

<span data-ttu-id="fea6c-227">Příkladem použití předchozích funkce Node.js hello může být:</span><span class="sxs-lookup"><span data-stu-id="fea6c-227">An example using hello preceding Node.js function would be:</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

<span data-ttu-id="fea6c-228">Hello výsledek, který uděluje přístup tooall funkce pro device1, bude:</span><span class="sxs-lookup"><span data-stu-id="fea6c-228">hello result, which grants access tooall functionality for device1, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> <span data-ttu-id="fea6c-229">Ho je možné toogenerate token SAS pomocí rozhraní .NET hello [explorer zařízení] [ lnk-device-explorer] nástroje nebo hello různé platformy, na základě uzlu [iothub-explorer] [ lnk-iothub-explorer] nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="fea6c-229">It is possible toogenerate a SAS token using hello .NET [device explorer][lnk-device-explorer] tool or hello cross-platform, node-based [iothub-explorer][lnk-iothub-explorer] command-line utility.</span></span>

### <a name="use-a-shared-access-policy"></a><span data-ttu-id="fea6c-230">Použijte zásady sdíleného přístupu</span><span class="sxs-lookup"><span data-stu-id="fea6c-230">Use a shared access policy</span></span>

<span data-ttu-id="fea6c-231">Při vytváření tokenu z zásady sdíleného přístupu, nastavení hello `skn` název pole toohello hello zásad.</span><span class="sxs-lookup"><span data-stu-id="fea6c-231">When you create a token from a shared access policy, set hello `skn` field toohello name of hello policy.</span></span> <span data-ttu-id="fea6c-232">Tato zásada musí udělit hello **DeviceConnect** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="fea6c-232">This policy must grant hello **DeviceConnect** permission.</span></span>

<span data-ttu-id="fea6c-233">Hello dva základní scénáře pomocí funkce zařízení tooaccess zásady sdíleného přístupu jsou:</span><span class="sxs-lookup"><span data-stu-id="fea6c-233">hello two main scenarios for using shared access policies tooaccess device functionality are:</span></span>

* <span data-ttu-id="fea6c-234">[cloudové brány protokolu][lnk-endpoints],</span><span class="sxs-lookup"><span data-stu-id="fea6c-234">[cloud protocol gateways][lnk-endpoints],</span></span>
* <span data-ttu-id="fea6c-235">[token služby] [ lnk-custom-auth] používá schémat tooimplement vlastní ověřování.</span><span class="sxs-lookup"><span data-stu-id="fea6c-235">[token services][lnk-custom-auth] used tooimplement custom authentication schemes.</span></span>

<span data-ttu-id="fea6c-236">Od hello zásady sdíleného přístupu může potenciálně udělit přístup tooconnect jako jakékoli zařízení, že je důležité toouse hello správný identifikátor URI prostředku při vytváření tokenů zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-236">Since hello shared access policy can potentially grant access tooconnect as any device, it is important toouse hello correct resource URI when creating security tokens.</span></span> <span data-ttu-id="fea6c-237">Toto nastavení je obzvláště důležité pro token služby, které mají tooscope hello tokenu tooa konkrétní zařízení pomocí hello identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="fea6c-237">This setting is especially important for token services, which have tooscope hello token tooa specific device using hello resource URI.</span></span> <span data-ttu-id="fea6c-238">Tento bod je méně relevantní pro protokol brány jako jejich jsou již jehož prostřednictvím je umožněn přenos pro všechna zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-238">This point is less relevant for protocol gateways as they are already mediating traffic for all devices.</span></span>

<span data-ttu-id="fea6c-239">Jako příklad tokenu služby pomocí hello předem vytvořené sdílené zásady přístupu volat **zařízení** by vytvořit token s hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="fea6c-239">As an example, a token service using hello pre-created shared access policy called **device** would create a token with hello following parameters:</span></span>

* <span data-ttu-id="fea6c-240">identifikátor URI prostředku: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span><span class="sxs-lookup"><span data-stu-id="fea6c-240">resource URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span></span>
* <span data-ttu-id="fea6c-241">podpisový klíč: jeden z klíčů hello Dobrý den `device` zásad</span><span class="sxs-lookup"><span data-stu-id="fea6c-241">signing key: one of hello keys of hello `device` policy,</span></span>
* <span data-ttu-id="fea6c-242">Název zásady: `device`,</span><span class="sxs-lookup"><span data-stu-id="fea6c-242">policy name: `device`,</span></span>
* <span data-ttu-id="fea6c-243">kdykoli vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="fea6c-243">any expiration time.</span></span>

<span data-ttu-id="fea6c-244">Příkladem použití předchozích funkce Node.js hello může být:</span><span class="sxs-lookup"><span data-stu-id="fea6c-244">An example using hello preceding Node.js function would be:</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

<span data-ttu-id="fea6c-245">Hello výsledek, který uděluje přístup tooall funkce pro device1, bude:</span><span class="sxs-lookup"><span data-stu-id="fea6c-245">hello result, which grants access tooall functionality for device1, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

<span data-ttu-id="fea6c-246">Brána protokolu může používat stejný token pro všechna zařízení, jednoduše nastavení hello identifikátor URI příliš hello`myhub.azure-devices.net/devices`.</span><span class="sxs-lookup"><span data-stu-id="fea6c-246">A protocol gateway could use hello same token for all devices simply setting hello resource URI too`myhub.azure-devices.net/devices`.</span></span>

### <a name="use-security-tokens-from-service-components"></a><span data-ttu-id="fea6c-247">Použít tokeny zabezpečení ze součásti služby</span><span class="sxs-lookup"><span data-stu-id="fea6c-247">Use security tokens from service components</span></span>

<span data-ttu-id="fea6c-248">Součásti služby můžete vygenerovat pouze tokeny zabezpečení pomocí udělení hello příslušná oprávnění, jak je popsáno dříve zásady sdíleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="fea6c-248">Service components can only generate security tokens using shared access policies granting hello appropriate permissions as explained previously.</span></span>

<span data-ttu-id="fea6c-249">Tady je funkce služby hello zveřejněné na koncové body hello:</span><span class="sxs-lookup"><span data-stu-id="fea6c-249">Here is hello service functions exposed on hello endpoints:</span></span>

| <span data-ttu-id="fea6c-250">Koncový bod</span><span class="sxs-lookup"><span data-stu-id="fea6c-250">Endpoint</span></span> | <span data-ttu-id="fea6c-251">Funkce</span><span class="sxs-lookup"><span data-stu-id="fea6c-251">Functionality</span></span> |
| --- | --- |
| `{iot hub host name}/devices` |<span data-ttu-id="fea6c-252">Vytvoření, aktualizace, získání a odstranění identit zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-252">Create, update, retrieve, and delete device identities.</span></span> |
| `{iot hub host name}/messages/events` |<span data-ttu-id="fea6c-253">Příjem zpráv typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="fea6c-253">Receive device-to-cloud messages.</span></span> |
| `{iot hub host name}/servicebound/feedback` |<span data-ttu-id="fea6c-254">Přijímat zpětnou vazbu pro zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-254">Receive feedback for cloud-to-device messages.</span></span> |
| `{iot hub host name}/devicebound` |<span data-ttu-id="fea6c-255">Odesílání zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-255">Send cloud-to-device messages.</span></span> |

<span data-ttu-id="fea6c-256">Jako příklad služby generování pomocí hello předem vytvořené sdílené zásady přístupu volat **registryRead** by vytvořit token s hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="fea6c-256">As an example, a service generating using hello pre-created shared access policy called **registryRead** would create a token with hello following parameters:</span></span>

* <span data-ttu-id="fea6c-257">identifikátor URI prostředku: `{IoT hub name}.azure-devices.net/devices`,</span><span class="sxs-lookup"><span data-stu-id="fea6c-257">resource URI: `{IoT hub name}.azure-devices.net/devices`,</span></span>
* <span data-ttu-id="fea6c-258">podpisový klíč: jeden z klíčů hello Dobrý den `registryRead` zásad</span><span class="sxs-lookup"><span data-stu-id="fea6c-258">signing key: one of hello keys of hello `registryRead` policy,</span></span>
* <span data-ttu-id="fea6c-259">Název zásady: `registryRead`,</span><span class="sxs-lookup"><span data-stu-id="fea6c-259">policy name: `registryRead`,</span></span>
* <span data-ttu-id="fea6c-260">kdykoli vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="fea6c-260">any expiration time.</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

<span data-ttu-id="fea6c-261">Hello výsledek, který by udělit přístup tooread všechny identity zařízení, bude:</span><span class="sxs-lookup"><span data-stu-id="fea6c-261">hello result, which would grant access tooread all device identities, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a><span data-ttu-id="fea6c-262">Podporované certifikáty X.509</span><span class="sxs-lookup"><span data-stu-id="fea6c-262">Supported X.509 certificates</span></span>

<span data-ttu-id="fea6c-263">Službou IoT Hub můžete použít všechny tooauthenticate certifikát X.509 zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-263">You can use any X.509 certificate tooauthenticate a device with IoT Hub.</span></span> <span data-ttu-id="fea6c-264">Certifikáty patří:</span><span class="sxs-lookup"><span data-stu-id="fea6c-264">Certificates include:</span></span>

* <span data-ttu-id="fea6c-265">**Existující certifikát X.509**.</span><span class="sxs-lookup"><span data-stu-id="fea6c-265">**An existing X.509 certificate**.</span></span> <span data-ttu-id="fea6c-266">Zařízení již pravděpodobně certifikát X.509 s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="fea6c-266">A device may already have an X.509 certificate associated with it.</span></span> <span data-ttu-id="fea6c-267">Hello zařízení můžete použít tento certifikát tooauthenticate službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="fea6c-267">hello device can use this certificate tooauthenticate with IoT Hub.</span></span>
* <span data-ttu-id="fea6c-268">**A samoobslužné generované a X-509 certifikátu podepsaného svým držitelem**.</span><span class="sxs-lookup"><span data-stu-id="fea6c-268">**A self-generated and self-signed X-509 certificate**.</span></span> <span data-ttu-id="fea6c-269">Výrobce zařízení nebo interní nástroje pro nasazení můžete generovat tyto certifikáty a ukládat hello odpovídající privátní klíč (a certifikátu) na zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="fea6c-269">A device manufacturer or in-house deployer can generate these certificates and store hello corresponding private key (and certificate) on hello device.</span></span> <span data-ttu-id="fea6c-270">Můžete použít nástroje, jako [OpenSSL] [ lnk-openssl] a [Windows SelfSignedCertificate] [ lnk-selfsigned] nástroj pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="fea6c-270">You can use tools such as [OpenSSL][lnk-openssl] and [Windows SelfSignedCertificate][lnk-selfsigned] utility for this purpose.</span></span>
* <span data-ttu-id="fea6c-271">**Certifikační Autority podepsané certifikát X.509**.</span><span class="sxs-lookup"><span data-stu-id="fea6c-271">**CA-signed X.509 certificate**.</span></span> <span data-ttu-id="fea6c-272">tooidentify zařízení a pro ověření službou IoT Hub, můžete použít certifikát X.509, generovány a podepsaný pomocí certifikační autority (CA).</span><span class="sxs-lookup"><span data-stu-id="fea6c-272">tooidentify a device and authenticate it with IoT Hub, you can use an X.509 certificate generated and signed by a Certification Authority (CA).</span></span> <span data-ttu-id="fea6c-273">IoT Hub pouze ověřuje, že hello kryptografický otisk uvedené odpovídá hello nakonfigurovaný kryptografický otisk.</span><span class="sxs-lookup"><span data-stu-id="fea6c-273">IoT Hub only verifies that hello thumbprint presented matches hello configured thumbprint.</span></span> <span data-ttu-id="fea6c-274">IotHub nelze ověřit řetěz certifikátů hello.</span><span class="sxs-lookup"><span data-stu-id="fea6c-274">IotHub does not validate hello certificate chain.</span></span>

<span data-ttu-id="fea6c-275">Zařízení může použít certifikát X.509 nebo token zabezpečení pro ověřování, ale ne obojí.</span><span class="sxs-lookup"><span data-stu-id="fea6c-275">A device may either use an X.509 certificate or a security token for authentication, but not both.</span></span>

### <a name="register-an-x509-certificate-for-a-device"></a><span data-ttu-id="fea6c-276">Registrovat certifikát X.509 pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-276">Register an X.509 certificate for a device</span></span>

<span data-ttu-id="fea6c-277">Hello [sady SDK služby Azure IoT pro jazyk C#] [ lnk-service-sdk] (verze 1.0.8+) podporuje registraci zařízení, které používá certifikátu X.509. certifikát pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="fea6c-277">hello [Azure IoT Service SDK for C#][lnk-service-sdk] (version 1.0.8+) supports registering a device that uses an X.509 certificate for authentication.</span></span> <span data-ttu-id="fea6c-278">Jiná rozhraní API, jako je například importu a exportu zařízení také podporují certifikáty X.509.</span><span class="sxs-lookup"><span data-stu-id="fea6c-278">Other APIs such as import/export of devices also support X.509 certificates.</span></span>

### <a name="c-support"></a><span data-ttu-id="fea6c-279">C\# podpory</span><span class="sxs-lookup"><span data-stu-id="fea6c-279">C\# Support</span></span>

<span data-ttu-id="fea6c-280">Hello **RegistryManager** třída poskytuje programový způsob tooregister zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-280">hello **RegistryManager** class provides a programmatic way tooregister a device.</span></span> <span data-ttu-id="fea6c-281">Konkrétně hello **AddDeviceAsync** a **UpdateDeviceAsync** metody umožňují tooregister a aktualizujte zařízení v registru identit služby IoT Hub hello.</span><span class="sxs-lookup"><span data-stu-id="fea6c-281">In particular, hello **AddDeviceAsync** and **UpdateDeviceAsync** methods enable you tooregister and update a device in hello IoT Hub identity registry.</span></span> <span data-ttu-id="fea6c-282">Tyto dvě metody přijímají **zařízení** instance jako vstup.</span><span class="sxs-lookup"><span data-stu-id="fea6c-282">These two methods take a **Device** instance as input.</span></span> <span data-ttu-id="fea6c-283">Hello **zařízení** třída zahrnuje **ověřování** vlastnost, která vám umožní toospecify primární a sekundární X.509 kryptografické otisky certifikátu.</span><span class="sxs-lookup"><span data-stu-id="fea6c-283">hello **Device** class includes an **Authentication** property that allows you toospecify primary and secondary X.509 certificate thumbprints.</span></span> <span data-ttu-id="fea6c-284">Kryptografický otisk Hello představuje hodnotu hash SHA-1 certifikátu X.509 hello (uložené pomocí binární kódování DER).</span><span class="sxs-lookup"><span data-stu-id="fea6c-284">hello thumbprint represents a SHA-1 hash of hello X.509 certificate (stored using binary DER encoding).</span></span> <span data-ttu-id="fea6c-285">Máte možnost hello zadání primární kryptografický otisk nebo sekundární kryptografický otisk nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="fea6c-285">You have hello option of specifying a primary thumbprint or a secondary thumbprint or both.</span></span> <span data-ttu-id="fea6c-286">Primární a sekundární kryptografické otisky jsou podporované toohandle certifikát výměny scénáře.</span><span class="sxs-lookup"><span data-stu-id="fea6c-286">Primary and secondary thumbprints are supported toohandle certificate rollover scenarios.</span></span>

> [!NOTE]
> <span data-ttu-id="fea6c-287">IoT Hub nevyžaduje ani hello celý certifikát X.509, pouze kryptografický otisk hello uložit.</span><span class="sxs-lookup"><span data-stu-id="fea6c-287">IoT Hub does not require or store hello entire X.509 certificate, only hello thumbprint.</span></span>

<span data-ttu-id="fea6c-288">Zde je ukázka C\# code zařízení pomocí certifikátu X.509 tooregister fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="fea6c-288">Here is a sample C\# code snippet tooregister a device using an X.509 certificate:</span></span>

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

### <a name="use-an-x509-certificate-during-run-time-operations"></a><span data-ttu-id="fea6c-289">Použít certifikát X.509 během spuštění operací</span><span class="sxs-lookup"><span data-stu-id="fea6c-289">Use an X.509 certificate during run-time operations</span></span>

<span data-ttu-id="fea6c-290">Hello [zařízení Azure IoT SDK pro .NET] [ lnk-client-sdk] (verze 1.0.11+) podporuje hello certifikáty X.509.</span><span class="sxs-lookup"><span data-stu-id="fea6c-290">hello [Azure IoT device SDK for .NET][lnk-client-sdk] (version 1.0.11+) supports hello use of X.509 certificates.</span></span>

### <a name="c-support"></a><span data-ttu-id="fea6c-291">C\# podpory</span><span class="sxs-lookup"><span data-stu-id="fea6c-291">C\# Support</span></span>

<span data-ttu-id="fea6c-292">Hello třída **DeviceAuthenticationWithX509Certificate** podporuje hello vytvoření **DeviceClient** instance, používá certifikát X.509.</span><span class="sxs-lookup"><span data-stu-id="fea6c-292">hello class **DeviceAuthenticationWithX509Certificate** supports hello creation of **DeviceClient** instances using an X.509 certificate.</span></span> <span data-ttu-id="fea6c-293">certifikát X.509 Hello musí být ve formátu PFX (také nazývané PKCS #12) hello, která zahrnuje hello privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="fea6c-293">hello X.509 certificate must be in hello PFX (also called PKCS #12) format that includes hello private key.</span></span>

<span data-ttu-id="fea6c-294">Zde je fragment kódu ukázka:</span><span class="sxs-lookup"><span data-stu-id="fea6c-294">Here is a sample code snippet:</span></span>

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a><span data-ttu-id="fea6c-295">Ověřování vlastní zařízení</span><span class="sxs-lookup"><span data-stu-id="fea6c-295">Custom device authentication</span></span>

<span data-ttu-id="fea6c-296">Můžete použít hello IoT Hub [registru identit] [ lnk-identity-registry] tooconfigure podle zařízení zabezpečovací přihlašovací údaje a řízení přístupu pomocí [tokeny] [ lnk-sas-tokens] .</span><span class="sxs-lookup"><span data-stu-id="fea6c-296">You can use hello IoT Hub [identity registry][lnk-identity-registry] tooconfigure per-device security credentials and access control using [tokens][lnk-sas-tokens].</span></span> <span data-ttu-id="fea6c-297">Pokud řešení IoT už má vlastní identitu registru nebo ověřování schématu, zvažte vytvoření *token služby* toointegrate Tato infrastruktura službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="fea6c-297">If an IoT solution already has a custom identity registry and/or authentication scheme, consider creating a *token service* toointegrate this infrastructure with IoT Hub.</span></span> <span data-ttu-id="fea6c-298">Tímto způsobem můžete další funkce IoT ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-298">In this way, you can use other IoT features in your solution.</span></span>

<span data-ttu-id="fea6c-299">Token služby je vlastní Cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="fea6c-299">A token service is a custom cloud service.</span></span> <span data-ttu-id="fea6c-300">Používá služby IoT Hub *sdílené zásady přístupu* s **DeviceConnect** oprávnění toocreate *obor zařízení* tokeny.</span><span class="sxs-lookup"><span data-stu-id="fea6c-300">It uses an IoT Hub *shared access policy* with **DeviceConnect** permissions toocreate *device-scoped* tokens.</span></span> <span data-ttu-id="fea6c-301">Tyto tokeny umožňují zařízení tooconnect tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fea6c-301">These tokens enable a device tooconnect tooyour IoT hub.</span></span>

![Kroky hello služby tokenů vzor][img-tokenservice]

<span data-ttu-id="fea6c-303">Zde jsou hlavní kroky hello hello služby tokenů vzoru:</span><span class="sxs-lookup"><span data-stu-id="fea6c-303">Here are hello main steps of hello token service pattern:</span></span>

1. <span data-ttu-id="fea6c-304">Vytvoření služby IoT Hub sdílených zásad přístupu s **DeviceConnect** oprávnění pro službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fea6c-304">Create an IoT Hub shared access policy with **DeviceConnect** permissions for your IoT hub.</span></span> <span data-ttu-id="fea6c-305">Tyto zásady můžete vytvořit v hello [portál Azure] [ lnk-management-portal] nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="fea6c-305">You can create this policy in hello [Azure portal][lnk-management-portal] or programmatically.</span></span> <span data-ttu-id="fea6c-306">token služba Hello používá tento tokeny hello toosign zásad, který vytváří.</span><span class="sxs-lookup"><span data-stu-id="fea6c-306">hello token service uses this policy toosign hello tokens it creates.</span></span>
1. <span data-ttu-id="fea6c-307">Když se zařízení potřebuje tooaccess služby IoT hub, vyžádá podepsaný token ze služby tokenu.</span><span class="sxs-lookup"><span data-stu-id="fea6c-307">When a device needs tooaccess your IoT hub, it requests a signed token from your token service.</span></span> <span data-ttu-id="fea6c-308">Hello zařízení můžete ověřit pomocí vaše vlastní identitu registru nebo ověřování schématu toodetermine hello identitu zařízení, že služba tokenu hello používá toocreate hello token.</span><span class="sxs-lookup"><span data-stu-id="fea6c-308">hello device can authenticate with your custom identity registry/authentication scheme toodetermine hello device identity that hello token service uses toocreate hello token.</span></span>
1. <span data-ttu-id="fea6c-309">Služba tokenu Hello vrátí token.</span><span class="sxs-lookup"><span data-stu-id="fea6c-309">hello token service returns a token.</span></span> <span data-ttu-id="fea6c-310">Hello token je vytvořená pomocí `/devices/{deviceId}` jako `resourceURI`, s `deviceId` jako zařízení hello ověřovaného.</span><span class="sxs-lookup"><span data-stu-id="fea6c-310">hello token is created by using `/devices/{deviceId}` as `resourceURI`, with `deviceId` as hello device being authenticated.</span></span> <span data-ttu-id="fea6c-311">Služba tokenu Hello používá hello sdíleného přístupu zásad tooconstruct hello token.</span><span class="sxs-lookup"><span data-stu-id="fea6c-311">hello token service uses hello shared access policy tooconstruct hello token.</span></span>
1. <span data-ttu-id="fea6c-312">zařízení Hello používá hello token přímo službou hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fea6c-312">hello device uses hello token directly with hello IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="fea6c-313">Můžete použít třídu .NET hello [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] nebo hello třída jazyka Java [IotHubServiceSasToken] [ lnk-java-sas] toocreate token ve službě tokenu.</span><span class="sxs-lookup"><span data-stu-id="fea6c-313">You can use hello .NET class [SharedAccessSignatureBuilder][lnk-dotnet-sas] or hello Java class [IotHubServiceSasToken][lnk-java-sas] toocreate a token in your token service.</span></span>

<span data-ttu-id="fea6c-314">Služba tokenu Hello můžete nastavit vypršení platnosti tokenu hello podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="fea6c-314">hello token service can set hello token expiration as desired.</span></span> <span data-ttu-id="fea6c-315">Když vyprší platnost tokenu hello, servery hello IoT hub hello připojení zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-315">When hello token expires, hello IoT hub severs hello device connection.</span></span> <span data-ttu-id="fea6c-316">Potom hello zařízení musíte požádat o nový token od služby tokenů hello.</span><span class="sxs-lookup"><span data-stu-id="fea6c-316">Then, hello device must request a new token from hello token service.</span></span> <span data-ttu-id="fea6c-317">Čas vypršení platnosti krátký zvyšuje zatížení hello hello zařízení a služby tokenů hello.</span><span class="sxs-lookup"><span data-stu-id="fea6c-317">A short expiry time increases hello load on both hello device and hello token service.</span></span>

<span data-ttu-id="fea6c-318">Pro zařízení tooconnect tooyour rozbočovače, je třeba stále přidat ji toohello registru identit služby IoT Hub – Přestože hello zařízení používá token a ne tooconnect klíče o zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-318">For a device tooconnect tooyour hub, you must still add it toohello IoT Hub identity registry — even though hello device is using a token and not a device key tooconnect.</span></span> <span data-ttu-id="fea6c-319">Proto můžete pokračovat v řízení přístupu podle zařízení toouse povolením nebo zakázáním identit zařízení ve hello [registru identit][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="fea6c-319">Therefore, you can continue toouse per-device access control by enabling or disabling device identities in hello [identity registry][lnk-identity-registry].</span></span> <span data-ttu-id="fea6c-320">Tento přístup snižuje rizika spojená hello tokeny pomocí vypršení platnosti dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="fea6c-320">This approach mitigates hello risks of using tokens with long expiry times.</span></span>

### <a name="comparison-with-a-custom-gateway"></a><span data-ttu-id="fea6c-321">Porovnání s vlastní bránu</span><span class="sxs-lookup"><span data-stu-id="fea6c-321">Comparison with a custom gateway</span></span>

<span data-ttu-id="fea6c-322">vzor služby tokenů Hello je, že hello doporučená tooimplement způsob schéma vlastní identitu registru nebo ověřování službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="fea6c-322">hello token service pattern is hello recommended way tooimplement a custom identity registry/authentication scheme with IoT Hub.</span></span> <span data-ttu-id="fea6c-323">Tento vzor je doporučená, protože IoT Hub pokračuje toohandle většina hello řešení provozu.</span><span class="sxs-lookup"><span data-stu-id="fea6c-323">This pattern is recommended because IoT Hub continues toohandle most of hello solution traffic.</span></span> <span data-ttu-id="fea6c-324">Ale pokud schéma vlastní ověřování hello tak vzájemně propojeny s protokolem hello, možná budete potřebovat *vlastní brány* tooprocess všechny hello provoz.</span><span class="sxs-lookup"><span data-stu-id="fea6c-324">However, if hello custom authentication scheme is so intertwined with hello protocol, you may require a *custom gateway* tooprocess all hello traffic.</span></span> <span data-ttu-id="fea6c-325">Příkladem takové situaci je pomocí[zabezpečení TLS (Transport Layer) a předsdílených klíčů (PSKs)][lnk-tls-psk].</span><span class="sxs-lookup"><span data-stu-id="fea6c-325">An example of such a scenario is using[Transport Layer Security (TLS) and pre-shared keys (PSKs)][lnk-tls-psk].</span></span> <span data-ttu-id="fea6c-326">Další informace najdete v tématu hello [brány protokolu] [ lnk-protocols] tématu.</span><span class="sxs-lookup"><span data-stu-id="fea6c-326">For more information, see hello [protocol gateway][lnk-protocols] topic.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="fea6c-327">Témata odkazů:</span><span class="sxs-lookup"><span data-stu-id="fea6c-327">Reference topics:</span></span>

<span data-ttu-id="fea6c-328">Hello následující odkazy na témata poskytují další informace o řízení přístupu tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fea6c-328">hello following reference topics provide you with more information about controlling access tooyour IoT hub.</span></span>

## <a name="iot-hub-permissions"></a><span data-ttu-id="fea6c-329">IoT Hub oprávnění</span><span class="sxs-lookup"><span data-stu-id="fea6c-329">IoT Hub permissions</span></span>

<span data-ttu-id="fea6c-330">Hello následující tabulka uvádí hello oprávnění můžete použít Centrum IoT tooyour toocontrol přístup.</span><span class="sxs-lookup"><span data-stu-id="fea6c-330">hello following table lists hello permissions you can use toocontrol access tooyour IoT hub.</span></span>

| <span data-ttu-id="fea6c-331">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="fea6c-331">Permission</span></span> | <span data-ttu-id="fea6c-332">Poznámky</span><span class="sxs-lookup"><span data-stu-id="fea6c-332">Notes</span></span> |
| --- | --- |
| <span data-ttu-id="fea6c-333">**RegistryRead**</span><span class="sxs-lookup"><span data-stu-id="fea6c-333">**RegistryRead**</span></span> |<span data-ttu-id="fea6c-334">Registr identit toohello uděluje přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-334">Grants read access toohello identity registry.</span></span> <span data-ttu-id="fea6c-335">Další informace najdete v tématu [registru identit][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="fea6c-335">For more information, see [Identity registry][lnk-identity-registry].</span></span> <br/><span data-ttu-id="fea6c-336">Toto oprávnění je používán back-end cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="fea6c-336">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="fea6c-337">**RegistryReadWrite**</span><span class="sxs-lookup"><span data-stu-id="fea6c-337">**RegistryReadWrite**</span></span> |<span data-ttu-id="fea6c-338">Uděluje oprávnění ke čtení a zápisu toohello registru identit.</span><span class="sxs-lookup"><span data-stu-id="fea6c-338">Grants read and write access toohello identity registry.</span></span> <span data-ttu-id="fea6c-339">Další informace najdete v tématu [registru identit][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="fea6c-339">For more information, see [Identity registry][lnk-identity-registry].</span></span> <br/><span data-ttu-id="fea6c-340">Toto oprávnění je používán back-end cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="fea6c-340">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="fea6c-341">**ServiceConnect**</span><span class="sxs-lookup"><span data-stu-id="fea6c-341">**ServiceConnect**</span></span> |<span data-ttu-id="fea6c-342">Uděluje přístup ke komunikaci služby směřujících toocloud a sledování koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="fea6c-342">Grants access toocloud service-facing communication and monitoring endpoints.</span></span> <br/><span data-ttu-id="fea6c-343">Uděluje oprávnění tooreceive zařízení cloud zprávy, odesílání zpráv typu cloud zařízení a načtení hello odpovídající potvrzování doručení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-343">Grants permission tooreceive device-to-cloud messages, send cloud-to-device messages, and retrieve hello corresponding delivery acknowledgments.</span></span> <br/><span data-ttu-id="fea6c-344">Uděluje oprávnění tooretrieve doručení potvrzení pro soubor odešle.</span><span class="sxs-lookup"><span data-stu-id="fea6c-344">Grants permission tooretrieve delivery acknowledgements for file uploads.</span></span> <br/><span data-ttu-id="fea6c-345">Uděluje oprávnění tooaccess zařízení dvojčata tooupdate značky a požadované vlastnosti načíst hlášené vlastnosti a spouštět dotazy.</span><span class="sxs-lookup"><span data-stu-id="fea6c-345">Grants permission tooaccess device twins tooupdate tags and desired properties, retrieve reported properties, and run queries.</span></span> <br/><span data-ttu-id="fea6c-346">Toto oprávnění je používán back-end cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="fea6c-346">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="fea6c-347">**DeviceConnect**</span><span class="sxs-lookup"><span data-stu-id="fea6c-347">**DeviceConnect**</span></span> |<span data-ttu-id="fea6c-348">Uděluje přístup k toodevice přístupem koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="fea6c-348">Grants access toodevice-facing endpoints.</span></span> <br/><span data-ttu-id="fea6c-349">Uděluje oprávnění toosend zařízení cloud zpráv a příjem zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-349">Grants permission toosend device-to-cloud messages and receive cloud-to-device messages.</span></span> <br/><span data-ttu-id="fea6c-350">Uděluje oprávnění tooperform nahrávání souborů ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-350">Grants permission tooperform file upload from a device.</span></span> <br/><span data-ttu-id="fea6c-351">Uděluje oprávnění tooreceive zařízení twin potřeby vlastnost oznámení a aktualizace zařízení twin hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fea6c-351">Grants permission tooreceive device twin desired property notifications and update device twin reported properties.</span></span> <br/><span data-ttu-id="fea6c-352">Uděluje oprávnění tooperform soubor odešle.</span><span class="sxs-lookup"><span data-stu-id="fea6c-352">Grants permission tooperform file uploads.</span></span> <br/><span data-ttu-id="fea6c-353">Toto oprávnění je používán zařízení.</span><span class="sxs-lookup"><span data-stu-id="fea6c-353">This permission is used by devices.</span></span> |

## <a name="additional-reference-material"></a><span data-ttu-id="fea6c-354">Odkaz na další materiály</span><span class="sxs-lookup"><span data-stu-id="fea6c-354">Additional reference material</span></span>

<span data-ttu-id="fea6c-355">Další témata referenční příručka vývojáře pro službu IoT Hub hello patří:</span><span class="sxs-lookup"><span data-stu-id="fea6c-355">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="fea6c-356">[Koncové body centra IoT] [ lnk-endpoints] popisuje hello různých koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.</span><span class="sxs-lookup"><span data-stu-id="fea6c-356">[IoT Hub endpoints][lnk-endpoints] describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="fea6c-357">[Omezování a kvóty] [ lnk-quotas] popisuje hello kvót a omezování chování, které se vztahují toohello služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="fea6c-357">[Throttling and quotas][lnk-quotas] describes hello quotas and throttling behaviors that apply toohello IoT Hub service.</span></span>
* <span data-ttu-id="fea6c-358">[Azure IoT zařízení a služby sady SDK] [ lnk-sdks] seznamy hello různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.</span><span class="sxs-lookup"><span data-stu-id="fea6c-358">[Azure IoT device and service SDKs][lnk-sdks] lists hello various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="fea6c-359">[IoT Hub dotazovací jazyk] [ lnk-query] popisuje hello dotazovací jazyk tooretrieve informace ze služby IoT Hub o úlohách a dvojčata zařízení můžete použít.</span><span class="sxs-lookup"><span data-stu-id="fea6c-359">[IoT Hub query language][lnk-query] describes hello query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="fea6c-360">[Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="fea6c-360">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fea6c-361">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fea6c-361">Next steps</span></span>

<span data-ttu-id="fea6c-362">Nyní jste se naučili, jak toocontrol přístup IoT Hub, může být zájem o hello následující témata Průvodce vývojáře IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="fea6c-362">Now you have learned how toocontrol access IoT Hub, you may be interested in hello following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="fea6c-363">[Používání stavu toosynchronize dvojčata zařízení a konfigurací][lnk-devguide-device-twins]</span><span class="sxs-lookup"><span data-stu-id="fea6c-363">[Use device twins toosynchronize state and configurations][lnk-devguide-device-twins]</span></span>
* <span data-ttu-id="fea6c-364">[Volání metody přímé na zařízení][lnk-devguide-directmethods]</span><span class="sxs-lookup"><span data-stu-id="fea6c-364">[Invoke a direct method on a device][lnk-devguide-directmethods]</span></span>
* <span data-ttu-id="fea6c-365">[Plánování úloh na několika zařízeních][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="fea6c-365">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="fea6c-366">Pokud chcete tootry některé z hello konceptů popsaných v tomto článku, může být zájem o hello následující kurzy IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="fea6c-366">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorials:</span></span>

* <span data-ttu-id="fea6c-367">[Začínáme s Azure IoT Hub][lnk-getstarted-tutorial]</span><span class="sxs-lookup"><span data-stu-id="fea6c-367">[Get started with Azure IoT Hub][lnk-getstarted-tutorial]</span></span>
* <span data-ttu-id="fea6c-368">[Jak toosend cloud zařízení zprávy s centrem IoT][lnk-c2d-tutorial]</span><span class="sxs-lookup"><span data-stu-id="fea6c-368">[How toosend cloud-to-device messages with IoT Hub][lnk-c2d-tutorial]</span></span>
* <span data-ttu-id="fea6c-369">[Jak tooprocess zpráv typu zařízení cloud IoT Hub][lnk-d2c-tutorial]</span><span class="sxs-lookup"><span data-stu-id="fea6c-369">[How tooprocess IoT Hub device-to-cloud messages][lnk-d2c-tutorial]</span></span>

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
