# <a name="secure-your-iot-deployment"></a><span data-ttu-id="482f1-101">Zabezpečení nasazení IoT</span><span class="sxs-lookup"><span data-stu-id="482f1-101">Secure your IoT deployment</span></span>
<span data-ttu-id="482f1-102">Tento článek obsahuje hello další úroveň podrobností pro zabezpečení infrastruktury založené na Azure IoT Internet věcí (IoT) hello.</span><span class="sxs-lookup"><span data-stu-id="482f1-102">This article provides hello next level of detail for securing hello Azure IoT-based Internet of Things (IoT) infrastructure.</span></span> <span data-ttu-id="482f1-103">Odkazuje tooimplementation úroveň podrobností pro konfiguraci a nasazení jednotlivých součástí.</span><span class="sxs-lookup"><span data-stu-id="482f1-103">It links tooimplementation level details for configuring and deploying each component.</span></span> <span data-ttu-id="482f1-104">Poskytuje taky porovnání a možnosti mezi různé konkurenční metody.</span><span class="sxs-lookup"><span data-stu-id="482f1-104">It also provides comparisons and choices between various competing methods.</span></span>

<span data-ttu-id="482f1-105">Zabezpečení nasazení Azure IoT hello je možné rozdělit do hello následující tři oblasti zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="482f1-105">Securing hello Azure IoT deployment can be divided into hello following three security areas:</span></span>

* <span data-ttu-id="482f1-106">**Zabezpečení zařízení**: zabezpečení zařízení IoT hello při nasazení v divoký hello.</span><span class="sxs-lookup"><span data-stu-id="482f1-106">**Device Security**: Securing hello IoT device while it is deployed in hello wild.</span></span>
* <span data-ttu-id="482f1-107">**Zabezpečení připojení**: zajištění všechna data přenášená mezi hello zařízení IoT a IoT Hub je důvěrný a manipulací.</span><span class="sxs-lookup"><span data-stu-id="482f1-107">**Connection Security**: Ensuring all data transmitted between hello IoT device and IoT Hub is confidential and tamper-proof.</span></span>
* <span data-ttu-id="482f1-108">**Zabezpečení cloudu**: poskytnutím prostředků toosecure dat prochází přes, a je uložen v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="482f1-108">**Cloud Security**: Providing a means toosecure data while it moves through, and is stored in hello cloud.</span></span>

![Tři oblasti zabezpečení][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a><span data-ttu-id="482f1-110">Zabezpečené zřizování zařízení a ověřování</span><span class="sxs-lookup"><span data-stu-id="482f1-110">Secure device provisioning and authentication</span></span>
<span data-ttu-id="482f1-111">Hello Azure IoT Suite zabezpečuje zařízení IoT pomocí hello následující dvě metody:</span><span class="sxs-lookup"><span data-stu-id="482f1-111">hello Azure IoT Suite secures IoT devices by hello following two methods:</span></span>

* <span data-ttu-id="482f1-112">Tím, že pro každé zařízení, které mohou být využívána hello zařízení toocommunicate s hello IoT Hub poskytuje jedinečnou identitu klíč (tokeny zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="482f1-112">By providing a unique identity key (security tokens) for each device, which can be used by hello device toocommunicate with hello IoT Hub.</span></span>
* <span data-ttu-id="482f1-113">Pomocí na zařízení [certifikát X.509] [ lnk-x509] a privátní klíče jako znamená tooauthenticate hello zařízení toohello IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="482f1-113">By using an on-device [X.509 certificate][lnk-x509] and private key as a means tooauthenticate hello device toohello IoT Hub.</span></span> <span data-ttu-id="482f1-114">Tato metoda ověřování zajišťuje, že hello privátní klíč na zařízení hello nezná mimo hello zařízení kdykoli, poskytuje vyšší úroveň zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="482f1-114">This authentication method ensures that hello private key on hello device is not known outside hello device at any time, providing a higher level of security.</span></span>

<span data-ttu-id="482f1-115">Metoda token zabezpečení Hello poskytuje ověření pro každé volání provedené hello zařízení tooIoT Centrum tím, že přidružíte hello symetrického klíče tooeach volání.</span><span class="sxs-lookup"><span data-stu-id="482f1-115">hello security token method provides authentication for each call made by hello device tooIoT Hub by associating hello symmetric key tooeach call.</span></span> <span data-ttu-id="482f1-116">Ověřování na základě X.509 umožňuje ověřování zařízení IoT ve fyzické vrstvě hello jako součást hello TLS připojení zařízení.</span><span class="sxs-lookup"><span data-stu-id="482f1-116">X.509-based authentication allows authentication of an IoT device at hello physical layer as part of hello TLS connection establishment.</span></span> <span data-ttu-id="482f1-117">Metoda na základě zabezpečení tokenu Hello můžete použít bez ověřování hello X.509, což je méně bezpečné vzor.</span><span class="sxs-lookup"><span data-stu-id="482f1-117">hello security-token-based method can be used without hello X.509 authentication which is a less secure pattern.</span></span> <span data-ttu-id="482f1-118">Hello volba mezi hello dvě metody je primárně závisí podle míry budou zabezpečené hello ověřování zařízení potřebuje toobe a dostupnost zabezpečeného úložiště na zařízení hello (toostore hello privátní klíč bezpečně).</span><span class="sxs-lookup"><span data-stu-id="482f1-118">hello choice between hello two methods is primarily dictated by how secure hello device authentication needs toobe, and availability of secure storage on hello device (toostore hello private key securely).</span></span>

## <a name="iot-hub-security-tokens"></a><span data-ttu-id="482f1-119">Tokeny zabezpečení IoT Hub</span><span class="sxs-lookup"><span data-stu-id="482f1-119">IoT Hub security tokens</span></span>
<span data-ttu-id="482f1-120">IoT Hub používá zabezpečení tokeny tooauthenticate zařízení a služeb tooavoid, odesílání klíčů v síti hello.</span><span class="sxs-lookup"><span data-stu-id="482f1-120">IoT Hub uses security tokens tooauthenticate devices and services tooavoid sending keys on hello network.</span></span> <span data-ttu-id="482f1-121">Kromě toho mají omezenou dobu platnosti a obor tokeny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="482f1-121">Additionally, security tokens are limited in time validity and scope.</span></span> <span data-ttu-id="482f1-122">Sady SDK služby Azure IoT automaticky generovat tokeny bez nutnosti žádnou zvláštní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="482f1-122">Azure IoT SDKs automatically generate tokens without requiring any special configuration.</span></span> <span data-ttu-id="482f1-123">Některé scénáře, ale vyžaduje toogenerate hello uživatele a používá tokeny zabezpečení přímo.</span><span class="sxs-lookup"><span data-stu-id="482f1-123">Some scenarios, however, require hello user toogenerate and use security tokens directly.</span></span> <span data-ttu-id="482f1-124">Mezi ně patří hello přímého použití hello MQTT, AMQP nebo HTTP ploch nebo hello implementace vzoru hello služby tokenů.</span><span class="sxs-lookup"><span data-stu-id="482f1-124">These include hello direct use of hello MQTT, AMQP, or HTTP surfaces, or hello implementation of hello token service pattern.</span></span>

<span data-ttu-id="482f1-125">Další informace o struktuře hello hello token zabezpečení a jeho použití naleznete v hello následující články:</span><span class="sxs-lookup"><span data-stu-id="482f1-125">More details on hello structure of hello security token and its usage can be found in hello following articles:</span></span>

* <span data-ttu-id="482f1-126">[Struktura tokenu zabezpečení][lnk-security-tokens]</span><span class="sxs-lookup"><span data-stu-id="482f1-126">[Security token structure][lnk-security-tokens]</span></span>
* <span data-ttu-id="482f1-127">[Pomocí SAS tokeny jako zařízení][lnk-sas-tokens]</span><span class="sxs-lookup"><span data-stu-id="482f1-127">[Using SAS tokens as a device][lnk-sas-tokens]</span></span>

<span data-ttu-id="482f1-128">Každé centrum IoT má [registru identit] [ lnk-identity-registry] , může být prostředky na zařízení použité toocreate v hello služby, jako je například fronty, která obsahuje neukládají zprávy typu cloud zařízení a toohello tooallow přístup zařízení přístupem koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="482f1-128">Each IoT Hub has an [identity registry][lnk-identity-registry] that can be used toocreate per-device resources in hello service, such as a queue that contains in-flight cloud-to-device messages, and tooallow access toohello device-facing endpoints.</span></span> <span data-ttu-id="482f1-129">Hello registru identit služby IoT Hub poskytuje zabezpečené úložiště identit zařízení a zabezpečení klíčů pro řešení.</span><span class="sxs-lookup"><span data-stu-id="482f1-129">hello IoT Hub identity registry provides secure storage of device identities and security keys for a solution.</span></span> <span data-ttu-id="482f1-130">Jednotlivé nebo skupiny identit zařízení mohou být přidány tooan povolit seznam nebo seznamy bloku, povolení plnou kontrolu nad přístup k zařízení.</span><span class="sxs-lookup"><span data-stu-id="482f1-130">Individual or groups of device identities can be added tooan allow list, or a block list, enabling complete control over device access.</span></span> <span data-ttu-id="482f1-131">Hello následující články poskytují další podrobnosti o hello struktura registru identit hello a podporované operace.</span><span class="sxs-lookup"><span data-stu-id="482f1-131">hello following articles provide more details on hello structure of hello identity registry and supported operations.</span></span>

<span data-ttu-id="482f1-132">[IoT Hub podporuje protokoly, například MQTT, AMQP a HTTP][lnk-protocols].</span><span class="sxs-lookup"><span data-stu-id="482f1-132">[IoT Hub supports protocols such as MQTT, AMQP, and HTTP][lnk-protocols].</span></span> <span data-ttu-id="482f1-133">Každý z těchto protokolů jinak použijte tokeny zabezpečení z tooIoT zařízení IoT hello rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="482f1-133">Each of these protocols use security tokens from hello IoT device tooIoT Hub differently:</span></span>

* <span data-ttu-id="482f1-134">AMQP: SASL prostý a založené na deklaracích AMQP zabezpečení ({policyName}@sas.root. { iothubName} v případě hello tokenů úrovni centra IoT; {deviceId} v případě zařízení obor tokeny).</span><span class="sxs-lookup"><span data-stu-id="482f1-134">AMQP: SASL PLAIN and AMQP Claims-based security ({policyName}@sas.root.{iothubName} in hello case of IoT hub-level tokens; {deviceId} in case of device-scoped tokens).</span></span>
* <span data-ttu-id="482f1-135">MQTT: Připojení paketu používá {deviceId} jako hello {ClientId}, {IoThubhostname} / {deviceId} v hello **uživatelské jméno** pole a SAS token v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="482f1-135">MQTT: CONNECT packet uses {deviceId} as hello {ClientId}, {IoThubhostname}/{deviceId} in hello **Username** field and a SAS token in hello **Password** field.</span></span>
* <span data-ttu-id="482f1-136">HTTP: Je platný token v hlavičce autorizace požadavku hello.</span><span class="sxs-lookup"><span data-stu-id="482f1-136">HTTP: Valid token is in hello authorization request header.</span></span>

<span data-ttu-id="482f1-137">Registr identit služby IoT Hub může být použité tooconfigure podle zařízení zabezpečovací přihlašovací údaje a řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="482f1-137">IoT Hub identity registry can be used tooconfigure per-device security credentials and access control.</span></span> <span data-ttu-id="482f1-138">Však řešení IoT již má významné investice v [vlastní zařízení identity registru nebo ověřování schématu][lnk-custom-auth], lze ji integrovat do existující infrastruktury s centrem IoT vytvořením služby tokenů.</span><span class="sxs-lookup"><span data-stu-id="482f1-138">However, if an IoT solution already has a significant investment in a [custom device identity registry and/or authentication scheme][lnk-custom-auth], it can be integrated into an existing infrastructure with IoT Hub by creating a token service.</span></span>

### <a name="x509-certificate-based-device-authentication"></a><span data-ttu-id="482f1-139">Ověřování zařízení na základě certifikátu X.509</span><span class="sxs-lookup"><span data-stu-id="482f1-139">X.509 certificate-based device authentication</span></span>
<span data-ttu-id="482f1-140">Hello použití [zařízení na základě certifikátu X.509] [ lnk-use-x509] a jeho přidružený privátní a veřejné klíče dvojice povoluje další ověřování ve fyzické vrstvě hello.</span><span class="sxs-lookup"><span data-stu-id="482f1-140">hello use of a [device-based X.509 certificate][lnk-use-x509] and its associated private and public key pair allows additional authentication at hello physical layer.</span></span> <span data-ttu-id="482f1-141">privátní klíč Hello je bezpečně uloženo na hello zařízení a není zjistitelný mimo hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="482f1-141">hello private key is stored securely in hello device and is not discoverable outside hello device.</span></span> <span data-ttu-id="482f1-142">certifikát X.509 Hello obsahuje informace o hello zařízení, jako je například ID zařízení a další podrobnosti organizace.</span><span class="sxs-lookup"><span data-stu-id="482f1-142">hello X.509 certificate contains information about hello device, such as device ID, and other organizational details.</span></span> <span data-ttu-id="482f1-143">Podpis certifikátu hello je generována pomocí hello privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="482f1-143">A signature of hello certificate is generated by using hello private key.</span></span>

<span data-ttu-id="482f1-144">Zřizování toku vysoké úrovně zařízení:</span><span class="sxs-lookup"><span data-stu-id="482f1-144">High-level device provisioning flow:</span></span>

* <span data-ttu-id="482f1-145">Přidružte identifikátor tooa fyzického zařízení s – identitu zařízení a/nebo zařízení přidružených toohello certifikát X.509 během zařízení výrobní nebo uvedení do provozu.</span><span class="sxs-lookup"><span data-stu-id="482f1-145">Associate an identifier tooa physical device – device identity and/or X.509 certificate associated toohello device during device manufacturing or commissioning.</span></span>
* <span data-ttu-id="482f1-146">Vytvořte záznam odpovídající identity ve IoT Hub – identitu zařízení a informace o přidružených zařízení v registru identit služby IoT Hub hello.</span><span class="sxs-lookup"><span data-stu-id="482f1-146">Create a corresponding identity entry in IoT Hub – device identity and associated device information in hello IoT Hub identity registry.</span></span>
* <span data-ttu-id="482f1-147">Kryptografický otisk certifikátu X.509 bezpečně uložte v registru identit služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="482f1-147">Securely store X.509 certificate thumbprint in IoT Hub identity registry.</span></span>

### <a name="root-certificate-on-device"></a><span data-ttu-id="482f1-148">Kořenový certifikát na zařízení</span><span class="sxs-lookup"><span data-stu-id="482f1-148">Root certificate on device</span></span>
<span data-ttu-id="482f1-149">Při navazování připojení TLS zabezpečené službou IoT Hub, ověřuje hello zařízení IoT pomocí kořenový certifikát, který je součástí hello zařízení SDK služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="482f1-149">While establishing a secure TLS connection with IoT Hub, hello IoT device authenticates IoT Hub using a root certificate which is part of hello device SDK.</span></span> <span data-ttu-id="482f1-150">Pro klienta hello C sady SDK hello certifikát se nachází ve složce hello "\\c\\certifikátů" v kořenovém úložišti hello hello.</span><span class="sxs-lookup"><span data-stu-id="482f1-150">For hello C client SDK hello certificate is located under hello folder "\\c\\certs" under hello root of hello repo.</span></span> <span data-ttu-id="482f1-151">I když tyto kořenových certifikátů je dlouhodobé, jsou stále může vyprší nebo odvolat.</span><span class="sxs-lookup"><span data-stu-id="482f1-151">Though these root certificates are long-lived, they still may expire or be revoked.</span></span> <span data-ttu-id="482f1-152">Pokud neexistuje žádný způsob aktualizace hello certifikát na zařízení hello hello, že zařízení nemusí být možné připojit toosubsequently toohello IoT Hub (nebo jiné cloudové služby).</span><span class="sxs-lookup"><span data-stu-id="482f1-152">If there is no way of updating hello certificate on hello device, hello device may not be able toosubsequently connect toohello IoT Hub (or any other cloud service).</span></span> <span data-ttu-id="482f1-153">Toto riziko bude efektivně snížilo s znamená tooupdate hello kořenový certifikát po nasazení zařízení IoT hello.</span><span class="sxs-lookup"><span data-stu-id="482f1-153">Having a means tooupdate hello root certificate once hello IoT device is deployed will effectively mitigate this risk.</span></span>

## <a name="securing-hello-connection"></a><span data-ttu-id="482f1-154">Zabezpečení připojení hello</span><span class="sxs-lookup"><span data-stu-id="482f1-154">Securing hello connection</span></span>
<span data-ttu-id="482f1-155">Připojení k Internetu mezi hello zařízení IoT a IoT Hub, je zabezpečena pomocí hello standard zabezpečení TLS (Transport Layer).</span><span class="sxs-lookup"><span data-stu-id="482f1-155">Internet connection between hello IoT device and IoT Hub is secured using hello Transport Layer Security (TLS) standard.</span></span> <span data-ttu-id="482f1-156">Azure IoT podporuje [TLS 1.2][lnk-tls12], TLS 1.1 a TLS 1.0, v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="482f1-156">Azure IoT supports [TLS 1.2][lnk-tls12], TLS 1.1 and TLS 1.0, in this order.</span></span> <span data-ttu-id="482f1-157">Podpora pro protokol TLS 1.0 je k dispozici pouze z důvodů zpětné kompatibility.</span><span class="sxs-lookup"><span data-stu-id="482f1-157">Support for TLS 1.0 is provided for backward compatibility only.</span></span> <span data-ttu-id="482f1-158">Vzhledem k tomu, že poskytuje nejvyšší zabezpečení hello se doporučuje toouse TLS 1.2.</span><span class="sxs-lookup"><span data-stu-id="482f1-158">It is recommended toouse TLS 1.2 since it provides hello most security.</span></span>

<span data-ttu-id="482f1-159">Azure IoT Suite podporuje hello následující šifrovací sady, v tomto pořadí.</span><span class="sxs-lookup"><span data-stu-id="482f1-159">Azure IoT Suite supports hello following Cipher Suites, in this order.</span></span>

| <span data-ttu-id="482f1-160">Šifrovací sada</span><span class="sxs-lookup"><span data-stu-id="482f1-160">Cipher Suite</span></span> | <span data-ttu-id="482f1-161">Délka</span><span class="sxs-lookup"><span data-stu-id="482f1-161">Length</span></span> |
| --- | --- |
| <span data-ttu-id="482f1-162">Protokol TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 secp384r1 ECDH (0xc028) (ekvalizéru</span><span class="sxs-lookup"><span data-stu-id="482f1-162">TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (eq.</span></span> <span data-ttu-id="482f1-163">FS 7680 bits RSA)</span><span class="sxs-lookup"><span data-stu-id="482f1-163">7680 bits RSA) FS</span></span> |<span data-ttu-id="482f1-164">256</span><span class="sxs-lookup"><span data-stu-id="482f1-164">256</span></span> |
| <span data-ttu-id="482f1-165">Protokol TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (ekvalizéru</span><span class="sxs-lookup"><span data-stu-id="482f1-165">TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (eq.</span></span> <span data-ttu-id="482f1-166">FS 3072 bits RSA)</span><span class="sxs-lookup"><span data-stu-id="482f1-166">3072 bits RSA) FS</span></span> |<span data-ttu-id="482f1-167">128</span><span class="sxs-lookup"><span data-stu-id="482f1-167">128</span></span> |
| <span data-ttu-id="482f1-168">Protokol TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (ekvalizéru</span><span class="sxs-lookup"><span data-stu-id="482f1-168">TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq.</span></span> <span data-ttu-id="482f1-169">FS 7680 bits RSA)</span><span class="sxs-lookup"><span data-stu-id="482f1-169">7680 bits RSA) FS</span></span> |<span data-ttu-id="482f1-170">256</span><span class="sxs-lookup"><span data-stu-id="482f1-170">256</span></span> |
| <span data-ttu-id="482f1-171">Protokol TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (ekvalizéru</span><span class="sxs-lookup"><span data-stu-id="482f1-171">TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq.</span></span> <span data-ttu-id="482f1-172">FS 3072 bits RSA)</span><span class="sxs-lookup"><span data-stu-id="482f1-172">3072 bits RSA) FS</span></span> |<span data-ttu-id="482f1-173">128</span><span class="sxs-lookup"><span data-stu-id="482f1-173">128</span></span> |
| <span data-ttu-id="482f1-174">Protokol TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d)</span><span class="sxs-lookup"><span data-stu-id="482f1-174">TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d)</span></span> |<span data-ttu-id="482f1-175">256</span><span class="sxs-lookup"><span data-stu-id="482f1-175">256</span></span> |
| <span data-ttu-id="482f1-176">Protokol TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256 (0x9c)</span><span class="sxs-lookup"><span data-stu-id="482f1-176">TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256 (0x9c)</span></span> |<span data-ttu-id="482f1-177">128</span><span class="sxs-lookup"><span data-stu-id="482f1-177">128</span></span> |
| <span data-ttu-id="482f1-178">Protokol TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256 (0x3d)</span><span class="sxs-lookup"><span data-stu-id="482f1-178">TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256 (0x3d)</span></span> |<span data-ttu-id="482f1-179">256</span><span class="sxs-lookup"><span data-stu-id="482f1-179">256</span></span> |
| <span data-ttu-id="482f1-180">Protokol TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0x3c)</span><span class="sxs-lookup"><span data-stu-id="482f1-180">TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0x3c)</span></span> |<span data-ttu-id="482f1-181">128</span><span class="sxs-lookup"><span data-stu-id="482f1-181">128</span></span> |
| <span data-ttu-id="482f1-182">Protokol TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA (0x35)</span><span class="sxs-lookup"><span data-stu-id="482f1-182">TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA (0x35)</span></span> |<span data-ttu-id="482f1-183">256</span><span class="sxs-lookup"><span data-stu-id="482f1-183">256</span></span> |
| <span data-ttu-id="482f1-184">Protokol TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f)</span><span class="sxs-lookup"><span data-stu-id="482f1-184">TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f)</span></span> |<span data-ttu-id="482f1-185">128</span><span class="sxs-lookup"><span data-stu-id="482f1-185">128</span></span> |
| <span data-ttu-id="482f1-186">Protokol TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA (0xa)</span><span class="sxs-lookup"><span data-stu-id="482f1-186">TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA (0xa)</span></span> |<span data-ttu-id="482f1-187">112</span><span class="sxs-lookup"><span data-stu-id="482f1-187">112</span></span> |

## <a name="securing-hello-cloud"></a><span data-ttu-id="482f1-188">Zabezpečení cloudu hello</span><span class="sxs-lookup"><span data-stu-id="482f1-188">Securing hello cloud</span></span>
<span data-ttu-id="482f1-189">Azure IoT Hub umožňuje definice [zásad řízení přístupu] [ lnk-protocols] pro každý klíč zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="482f1-189">Azure IoT Hub allows definition of [access control policies][lnk-protocols] for each security key.</span></span> <span data-ttu-id="482f1-190">Používá hello následující sadu oprávnění toogrant přístup tooeach koncových bodů služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="482f1-190">It uses hello following set of permissions toogrant access tooeach of IoT Hub's endpoints.</span></span> <span data-ttu-id="482f1-191">Oprávnění omezit přístup tooan hello, IoT Hub v závislosti na funkcích.</span><span class="sxs-lookup"><span data-stu-id="482f1-191">Permissions limit hello access tooan IoT Hub based on functionality.</span></span>

* <span data-ttu-id="482f1-192">**RegistryRead**.</span><span class="sxs-lookup"><span data-stu-id="482f1-192">**RegistryRead**.</span></span> <span data-ttu-id="482f1-193">Registr identit toohello uděluje přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="482f1-193">Grants read access toohello identity registry.</span></span> <span data-ttu-id="482f1-194">Další informace najdete v tématu [registru identit][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="482f1-194">For more information, see [identity registry][lnk-identity-registry].</span></span>
* <span data-ttu-id="482f1-195">**RegistryReadWrite**.</span><span class="sxs-lookup"><span data-stu-id="482f1-195">**RegistryReadWrite**.</span></span> <span data-ttu-id="482f1-196">Uděluje oprávnění ke čtení a zápisu toohello registru identit.</span><span class="sxs-lookup"><span data-stu-id="482f1-196">Grants read and write access toohello identity registry.</span></span> <span data-ttu-id="482f1-197">Další informace najdete v tématu [registru identit][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="482f1-197">For more information, see [identity registry][lnk-identity-registry].</span></span>
* <span data-ttu-id="482f1-198">**ServiceConnect**.</span><span class="sxs-lookup"><span data-stu-id="482f1-198">**ServiceConnect**.</span></span> <span data-ttu-id="482f1-199">Uděluje přístup ke komunikaci služby směřujících toocloud a sledování koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="482f1-199">Grants access toocloud service-facing communication and monitoring endpoints.</span></span> <span data-ttu-id="482f1-200">Například udělí oprávnění tooback-end cloudu zpráv typu zařízení cloud services tooreceive, odesílání zpráv typu cloud zařízení a načíst hello odpovídající doručení potvrzování.</span><span class="sxs-lookup"><span data-stu-id="482f1-200">For example, it grants permission tooback-end cloud services tooreceive device-to-cloud messages, send cloud-to-device messages, and retrieve hello corresponding delivery acknowledgments.</span></span>
* <span data-ttu-id="482f1-201">**DeviceConnect**.</span><span class="sxs-lookup"><span data-stu-id="482f1-201">**DeviceConnect**.</span></span> <span data-ttu-id="482f1-202">Uděluje přístup k toodevice přístupem koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="482f1-202">Grants access toodevice-facing endpoints.</span></span> <span data-ttu-id="482f1-203">Například uděluje oprávnění zpráv typu zařízení cloud toosend a příjem zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="482f1-203">For example, it grants permission toosend device-to-cloud messages and receive cloud-to-device messages.</span></span> <span data-ttu-id="482f1-204">Toto oprávnění je používán zařízení.</span><span class="sxs-lookup"><span data-stu-id="482f1-204">This permission is used by devices.</span></span>

<span data-ttu-id="482f1-205">Existují dva způsoby tooobtain **DeviceConnect** oprávnění službou IoT Hub s [tokeny zabezpečení][lnk-sas-tokens]: pomocí klíče identity zařízení nebo sdílený přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="482f1-205">There are two ways tooobtain **DeviceConnect** permissions with IoT Hub with [security tokens][lnk-sas-tokens]: using a device identity key, or a shared access key.</span></span> <span data-ttu-id="482f1-206">Kromě toho je důležité toonote zveřejněného všechny funkce, které jsou přístupné ze zařízení záměrné u koncových bodů s předponou `/devices/{deviceId}`.</span><span class="sxs-lookup"><span data-stu-id="482f1-206">Moreover, it is important toonote that all functionality accessible from devices is exposed by design on endpoints with prefix `/devices/{deviceId}`.</span></span>

<span data-ttu-id="482f1-207">[Součásti služby můžete generovat jenom tokeny zabezpečení] [ lnk-service-tokens] pomocí sdílené zásady přístupu udělení hello příslušná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="482f1-207">[Service components can only generate security tokens][lnk-service-tokens] using shared access policies granting hello appropriate permissions.</span></span>

<span data-ttu-id="482f1-208">Azure IoT Hub a dalším službám, které může být součástí řešení hello povolit správu uživatele, kteří používají hello Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="482f1-208">Azure IoT Hub and other services which may be part of hello solution allow management of users using hello Azure Active Directory.</span></span>

<span data-ttu-id="482f1-209">Data ve službě Azure IoT Hub požity mohou být spotřebovávána různých služeb, jako je Azure Stream Analytics a úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="482f1-209">Data ingested by Azure IoT Hub can be consumed by a variety of services such as Azure Stream Analytics and Azure blob storage.</span></span> <span data-ttu-id="482f1-210">Tyto služby umožňují přístup pro správu.</span><span class="sxs-lookup"><span data-stu-id="482f1-210">These services allow management access.</span></span> <span data-ttu-id="482f1-211">Další informace o těchto službách a k dispozici následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="482f1-211">Read more about these services and available options below:</span></span>

* <span data-ttu-id="482f1-212">[Azure DocumentDB][lnk-docdb]: škálovatelné a plně indexované databázová služba pro částečně strukturovaná data, která spravuje metadata pro zařízení hello zřídíte, jako je například atributy, konfiguraci a vlastnosti zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="482f1-212">[Azure DocumentDB][lnk-docdb]: A scalable, fully-indexed database service for semi-structured data that manages metadata for hello devices you provision, such as attributes, configuration, and security properties.</span></span> <span data-ttu-id="482f1-213">DocumentDB nabízí vysoce výkonné a vysokou propustností zpracování, bez ohledu na schéma indexování dat a bohaté rozhraní SQL.</span><span class="sxs-lookup"><span data-stu-id="482f1-213">DocumentDB offers high-performance and high-throughput processing, schema-agnostic indexing of data, and a rich SQL query interface.</span></span>
* <span data-ttu-id="482f1-214">[Azure Stream Analytics][lnk-asa]: streamu v reálném čase zpracování hello cloudu, který umožňuje vám toorapidly vývoji a nasazení nízkými náklady analytics řešení toouncover přehledy v reálném čase ze zařízení, senzorů, infrastruktury a aplikace.</span><span class="sxs-lookup"><span data-stu-id="482f1-214">[Azure Stream Analytics][lnk-asa]: Real-time stream processing in hello cloud that enables you toorapidly develop and deploy a low-cost analytics solution toouncover real-time insights from devices, sensors, infrastructure, and applications.</span></span> <span data-ttu-id="482f1-215">Hello data z tato plně spravovaná služba můžete škálovat tooany svazku, zatímco stále dosahuje vysoké propustnosti, s nízkou latencí a odolnost proti chybám.</span><span class="sxs-lookup"><span data-stu-id="482f1-215">hello data from this fully-managed service can scale tooany volume while still achieving high throughput, low latency, and resiliency.</span></span>
* <span data-ttu-id="482f1-216">[Azure App Services][lnk-appservices]: cloudové platformy toobuild výkonné webové a mobilní aplikace, které se připojují toodata kdekoli; v cloudu hello nebo místně.</span><span class="sxs-lookup"><span data-stu-id="482f1-216">[Azure App Services][lnk-appservices]: A cloud platform toobuild powerful web and mobile apps that connect toodata anywhere; in hello cloud or on-premises.</span></span> <span data-ttu-id="482f1-217">Vytvářejte poutavé mobilní aplikace pro iOS, Android a Windows.</span><span class="sxs-lookup"><span data-stu-id="482f1-217">Build engaging mobile apps for iOS, Android, and Windows.</span></span> <span data-ttu-id="482f1-218">Integrate váš Software jako služba (SaaS) a podnikové aplikace s toodozens připojení se na pole cloudových služeb a podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="482f1-218">Integrate with your Software as a Service (SaaS) and enterprise applications with out-of-the-box connectivity toodozens of cloud-based services and enterprise applications.</span></span> <span data-ttu-id="482f1-219">Kód v váš oblíbený jazyk a IDE toobuild webové aplikace (.NET, Node.js, PHP, Python nebo Java) a rozhraní API rychlejší než kdy dřív.</span><span class="sxs-lookup"><span data-stu-id="482f1-219">Code in your favorite language and IDE (.NET, Node.js, PHP, Python, or Java) toobuild web apps and APIs faster than ever.</span></span>
* <span data-ttu-id="482f1-220">[Služba Logic Apps][lnk-logicapps]: hello funkce Logic Apps služby Azure App Service pomáhá integrovat IoT řešení tooyour existující-obchodní systémy a automatizovat pracovní postupy.</span><span class="sxs-lookup"><span data-stu-id="482f1-220">[Logic Apps][lnk-logicapps]: hello Logic Apps feature of Azure App Service helps integrate your IoT solution tooyour existing line-of-business systems and automate workflow processes.</span></span> <span data-ttu-id="482f1-221">Služba Logic Apps umožňuje vývojářům toodesign pracovních, které začínají spouštěčem událostí a provádějí sérii kroků – pravidla a akce, které pomocí výkonné konektory toointegrate své obchodní procesy.</span><span class="sxs-lookup"><span data-stu-id="482f1-221">Logic Apps enables developers toodesign workflows that start from a trigger and then execute a series of steps—rules and actions that use powerful connectors toointegrate with your business processes.</span></span> <span data-ttu-id="482f1-222">Služba Logic Apps nabízí připojení se na pole tooa velká ekosystém SaaS, cloudových a místních aplikací.</span><span class="sxs-lookup"><span data-stu-id="482f1-222">Logic Apps offers out-of-the-box connectivity tooa vast ecosystem of SaaS, cloud-based, and on-premises applications.</span></span>
* <span data-ttu-id="482f1-223">[Úložiště objektů blob Azure][lnk-blob]: spolehlivé a ekonomické cloudové úložiště pro data hello, že vaše zařízení odesílají toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="482f1-223">[Azure blob storage][lnk-blob]: Reliable, economical cloud storage for hello data that your devices send toohello cloud.</span></span>

## <a name="conclusion"></a><span data-ttu-id="482f1-224">Závěr</span><span class="sxs-lookup"><span data-stu-id="482f1-224">Conclusion</span></span>
<span data-ttu-id="482f1-225">Tento článek obsahuje přehled implementace úroveň podrobností pro navrhování a nasazení infrastruktury IoT pomocí Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="482f1-225">This article provides overview of implementation level details for designing and deploying an IoT infrastructure using Azure IoT.</span></span> <span data-ttu-id="482f1-226">Konfigurace jednotlivých součástí toobe zabezpečené je klíč v zabezpečení hello celé infrastruktury IoT.</span><span class="sxs-lookup"><span data-stu-id="482f1-226">Configuring each component toobe secure is key in securing hello overall IoT infrastructure.</span></span> <span data-ttu-id="482f1-227">k dispozici v Azure IoT rozhodnutích při návrhu Hello zadejte určité úrovně flexibilitu a možnost výběru; Každý výběr však může mít vliv na zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="482f1-227">hello design choices available in Azure IoT provide some level of flexibility and choice; however, each choice may have security implications.</span></span> <span data-ttu-id="482f1-228">Doporučuje se každý z těchto možností vyhodnotí vyhodnoťte riziko/náklady.</span><span class="sxs-lookup"><span data-stu-id="482f1-228">It is recommended that each of these choices be evaluated through a risk/cost assessment.</span></span>

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/