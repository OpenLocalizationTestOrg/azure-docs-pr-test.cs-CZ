---
title: "aaaConfigure SSL pro cloudové služby | Microsoft Docs"
description: "Zjistěte, jak toospecify koncový bod HTTPS pro webovou roli a jak tooupload protokolem SSL certifikát toosecure vaší aplikace. Tyto příklady použití hello portálu Azure."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 371ba204-48b6-41af-ab9f-ed1d64efe704
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: adegeo
ms.openlocfilehash: b19283bb7b0e95374f2ae9c3532eb1effc7d6a9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a>Konfigurace protokolu SSL pro všechny aplikace v Azure
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-configure-ssl-certificate-portal.md)
> * [Portál Azure Classic](cloud-services-configure-ssl-certificate.md)
>

Šifrování Secure Socket Layer (SSL) je metoda hello nejčastěji používaná zabezpečení dat posílaných v hello Internetu. Tato běžných úkolů popisuje jak toospecify koncový bod HTTPS pro webovou roli a jak tooupload protokolem SSL certifikát toosecure vaší aplikace.

> [!NOTE]
> Hello postupy v této úloze platí tooAzure cloudové služby; Aplikační služby, najdete v části [to](../app-service-web/web-sites-configure-ssl-certificate.md).
>

Tato úloha používá produkčním nasazení. Informace o používání pracovní nasazení se poskytuje na konci hello v tomto tématu.

Čtení [to](cloud-services-how-to-create-deploy-portal.md) první, pokud jste ještě nevytvořili cloudové služby.

[!INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a>Krok 1: Získání certifikátu protokolu SSL
tooconfigure SSL pro aplikaci, musíte nejdřív tooget certifikát SSL, která byla podepsána pomocí certifikační autoritou (CA), důvěryhodná třetí straně, která vystavuje certifikáty pro tento účel. Pokud není již nemáte, je třeba tooobtain jeden ze společnosti, která prodává certifikáty SSL.

certifikát Hello musí splňovat následující požadavky na certifikáty SSL v Azure hello:

* Hello certifikát musí obsahovat privátní klíč.
* Hello certifikátu musí být vytvořeny pro výměnu klíčů, exportovatelný tooa soubor Personal Information Exchange (.pfx).
* Hello názvu subjektu certifikátu musí odpovídat hello domény použít tooaccess hello cloudové služby. Nelze získat certifikát SSL od certifikační autority (CA) pro doménu cloudapp.net hello. Musíte získat toouse název vlastní domény při přístupu ke službě. Pokud budete požadovat certifikát od certifikační Autority, musí se název subjektu certifikátu hello odpovídat tooaccess název používaný hello vlastní domény aplikace. Například, pokud je váš vlastní název domény **contoso.com** by vyžádání certifikátu z certifikační Autority pro ***. contoso.com** nebo **www.contoso.com**.
* Hello certifikát musí používat minimálně 2048bitové šifrování.

Pro účely testování můžete [vytvořit](cloud-services-certs-create.md) a použít certifikát podepsaný svým držitelem. Certifikát podepsaný svým držitelem není ověřen pomocí certifikační Autority a hello cloudapp.net domény můžete použít jako adresu URL webu hello. Například hello následující úkol používá certifikát podepsaný svým držitelem, ve které hello je běžný název (CN) použitý v certifikátu hello **sslexample.cloudapp.net**.

V dalším kroku musí zahrnovat informace o certifikátu hello v definici služby a služby konfigurační soubory.

<a name="modify"> </a>

## <a name="step-2-modify-hello-service-definition-and-configuration-files"></a>Krok 2: Upravte soubory pro definice a konfigurace služby hello
Aplikace musí být nakonfigurované toouse hello certifikátu a koncový bod HTTPS musí být přidán. V důsledku toho hello definice služby a soubory konfigurace služby musí toobe aktualizovat.

1. Ve vašem vývojovém prostředí, otevřete soubor definice služby hello (CSDEF), přidejte **certifikáty** části v rámci hello **WebRole** části a zahrnovat hello následující informace certifikát (a zprostředkující certifikáty):

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Certificates>
            <Certificate name="SampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="My"
                        permissionLevel="limitedOrElevated" />
            <!-- IMPORTANT! Unless your certificate is either
            self-signed or signed directly by hello CA root, you
            must include all hello intermediate certificates
            here. You must list them here, even if they are
            not bound tooany endpoints. Failing toolist any of
            hello intermediate certificates may cause hard-to-reproduce
            interoperability problems on some clients.-->
            <Certificate name="CAForSampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="CA"
                        permissionLevel="limitedOrElevated" />
        </Certificates>
    ...
    </WebRole>
    ```

   Hello **certifikáty** oddíl definuje název hello naše certifikát, její umístění a název hello hello úložiště, kde se nachází.

   Oprávnění (`permisionLevel` atribut) může být sada tooone Dobrý den následující hodnoty:

   | Hodnota oprávnění | Popis |
   | --- | --- |
   | limitedOrElevated |**(Výchozí)**  Všechny procesy role můžete přístup hello privátní klíč. |
   | zvýšené |K privátní klíč hello přístup jenom procesy se zvýšenými oprávněními. |

2. V souboru definice služby, přidejte **InputEndpoint** v rámci hello **koncové body** části tooenable HTTPS:

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Endpoints>
            <InputEndpoint name="HttpsIn" protocol="https" port="443"
                certificate="SampleCertificate" />
        </Endpoints>
    ...
    </WebRole>
    ```

3. V souboru definice služby, přidejte **vazby** v rámci hello **lokality** části. Tento element přidá toomap vazba HTTPS webu tooyour koncový bod:

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="HttpsIn" endpointName="HttpsIn" />
                </Bindings>
            </Site>
        </Sites>
    ...
    </WebRole>
    ```

   Byly dokončeny všechny hello požadované změny toohello souboru definice služby; ale stále potřebujete informace o certifikátu hello tooadd hello služby konfiguračního souboru.
4. V konfiguračním souboru služby (CSCFG), ServiceConfiguration.Cloud.cscfg, přidejte **certifikáty** hodnotu s u vašeho certifikátu. Hello následující ukázka kódu obsahuje podrobnosti o hello **certifikáty** části, s výjimkou hodnota kryptografického otisku hello.

   ```xml
    <Role name="Deployment">
    ...
        <Certificates>
            <Certificate name="SampleCertificate"
                thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff"
                thumbprintAlgorithm="sha1" />
            <Certificate name="CAForSampleCertificate"
                thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc"
                thumbprintAlgorithm="sha1" />
        </Certificates>
    ...
    </Role>
    ```

(Tento příklad používá **sha1** pro hello kryptografický algoritmus. Zadejte hodnotu hello vhodnou pro váš certifikát kryptografický algoritmus.)

Teď, když hello služby definice a služby konfigurační soubory byly aktualizovány, balíček nasazení pro nahrávání tooAzure. Pokud používáte **cspack**, nepoužívejte **/generateConfigurationFile** příznak, jako který přepíše vloženého informace o certifikátu.

## <a name="step-3-upload-a-certificate"></a>Krok 3: Nahrát na server certifikát
Připojit toohello portál Azure a...

1. V hello **všechny prostředky** části hello portál, vyberte cloudové služby.

    ![Publikování cloudové služby](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. Klikněte na tlačítko **certifikáty**.

    ![Klikněte na ikonu certifikáty hello](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. Klikněte na tlačítko **nahrát** hello horní části oblasti certifikáty hello.

    ![Klikněte na položku nabídky nahrávání hello](media/cloud-services-configure-ssl-certificate-portal/Upload_menu.png)

4. Zadejte hello **soubor**, **heslo**, pak klikněte na tlačítko **nahrát** dole hello hello dat vstupní oblast.

## <a name="step-4-connect-toohello-role-instance-by-using-https"></a>Krok 4: Připojte instanci role toohello pomocí protokolu HTTPS
Teď, když vaše nasazení je spuštěná v Azure, můžete připojit tooit pomocí protokolu HTTPS.

1. Klikněte na tlačítko hello **adresa URL webu** tooopen až hello webového prohlížeče.

   ![Klikněte na tlačítko hello adresa URL webu](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2. Ve webovém prohlížeči, upravte hello odkaz toouse **https** místo **http**a potom navštivte stránku hello.

   > [!NOTE]
   > Pokud používáte certifikát podepsaný svým držitelem, když procházíte tooan koncový bod HTTPS, který je spojen s certifikát podepsaný svým držitelem hello může se zobrazit chyba certifikátu v prohlížeči hello. Používá certifikát podepsaný důvěryhodnou certifikační autoritou eliminuje tento problém; v hello té doby chybu můžete ignorovat hello. (Další možností je tooadd hello certifikát podepsaný svým držitelem toohello důvěryhodné certifikační autority úložišti certifikátů uživatele.)
   >
   >

   ![Verze preview webu](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

   > [!TIP]
   > Pokud chcete toouse SSL pro nasazení pracovní místo produkční nasazení, je nutné nejdříve toodetermine hello adresa URL použitá pro hello pracovní nasazení. Jakmile nasadila cloudové služby toohello URL hello pracovní prostředí je určen podle hello **ID nasazení** identifikátor GUID v tomto formátu:`https://deployment-id.cloudapp.net/`  
   >
   > Vytvoření certifikátu s hello běžný název (CN) rovna toohello na základě GUID adresou URL (například **328187776e774ceda8fc57609d404462.cloudapp.net**). Použití hello portálu tooadd hello certifikát tooyour připravený cloudové služby. Pak přidejte hello informace tooyour CSDEF a CSCFG soubory certifikátů, znovu zabalte aplikaci a aktualizovat vaše dvoufázové instalace toouse hello nový balíček pro nasazení.
   >

## <a name="next-steps"></a>Další kroky
* [Obecná konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).
* Zjistěte, jak příliš[nasazení cloudové služby](cloud-services-how-to-create-deploy-portal.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).
* [Správa služby cloud](cloud-services-how-to-manage-portal.md).
