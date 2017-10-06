---
title: "aaaConfigure SSL pro cloudové služby (klasické) | Microsoft Docs"
description: "Zjistěte, jak toospecify koncový bod HTTPS pro webovou roli a jak tooupload protokolem SSL certifikát toosecure vaší aplikace."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 4cbb7f38-7994-454d-b4f0-7259b558c766
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: adegeo
ms.openlocfilehash: a1ca031b98af49d371977a208ed24f6dc8ea2ac9
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
> 

Šifrování Secure Socket Layer (SSL) je metoda hello nejčastěji používaná zabezpečení dat posílaných v hello Internetu. Tato běžných úkolů popisuje jak toospecify koncový bod HTTPS pro webovou roli a jak tooupload protokolem SSL certifikát toosecure vaší aplikace.

> [!NOTE]
> Hello postupy v této úloze platí tooAzure cloudové služby; Aplikační služby, najdete v části [to](../app-service-web/web-sites-configure-ssl-certificate.md) článku.
> 
> 

Tato úloha používá produkčním nasazení. Informace o používání pracovní nasazení se poskytuje na konci hello v tomto tématu.

Čtení [to](cloud-services-how-to-create-deploy.md) nejprve článek, pokud jste ještě nevytvořili cloudové služby.

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

3. V souboru definice služby, přidejte **vazby** v rámci hello **lokality** části. V této části se přidá toomap vazba HTTPS webu tooyour koncový bod:
   
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
   
   Byly dokončeny všechny hello požadované změny toohello souboru definice služby, ale stále potřebujete informace o certifikátu hello tooadd hello služby konfiguračního souboru.
4. V konfiguračním souboru služby (CSCFG), ServiceConfiguration.Cloud.cscfg, přidejte **certifikáty** části v rámci hello **Role** části nahraďte hello ukázka kryptografický otisk hodnoty zobrazené níže s který vašeho certifikátu:
   
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

(hello předcházející příklad používá **sha1** pro hello kryptografický algoritmus. Zadejte hodnotu hello vhodnou pro váš certifikát kryptografický algoritmus.)

Teď, když hello služby definice a služby konfigurační soubory byly aktualizovány, balíček nasazení pro nahrávání tooAzure. Pokud používáte **cspack**, nepoužívejte **/generateConfigurationFile** příznak, jako který přepíše informací o certifikátu, můžete vložit.

## <a name="step-3-upload-a-certificate"></a>Krok 3: Nahrát na server certifikát
Balíček pro nasazení, musí být aktualizované toouse hello certifikátu a byl přidán koncový bod HTTPS. Nyní můžete nahrát hello certifikátů a balíčků tooAzure s hello portál Azure classic.

1. Přihlaste se toohello [portál Azure classic][Azure classic portal]. 
2. Klikněte na tlačítko **cloudové služby** na levém navigačním podokně hello.
3. Klikněte na tlačítko hello potřeby cloudové služby.
4. Klikněte na tlačítko hello **certifikáty** kartě.
   
    ![Klikněte na kartu certifikáty hello](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. Klikněte na tlačítko hello **nahrát** tlačítko.
   
    ![Odeslat](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. Zadejte hello **soubor**, **heslo**, pak klikněte na tlačítko **Complete** (hello zaškrtnutí).

## <a name="step-4-connect-toohello-role-instance-by-using-https"></a>Krok 4: Připojte instanci role toohello pomocí protokolu HTTPS
Teď, když vaše nasazení je spuštěná v Azure, můžete připojit tooit pomocí protokolu HTTPS.

1. V hello portál Azure classic, vyberte nasazení a pak klikněte na odkaz hello pod **adresa URL webu**.
   
   ![Určení adresy URL webu][2]
2. Ve webovém prohlížeči, upravte hello odkaz toouse **https** místo **http**a potom navštivte stránku hello.
   
   > [!NOTE]
   > Pokud používáte certifikát podepsaný svým držitelem, když procházíte tooan koncový bod HTTPS, který je spojen s certifikát podepsaný svým držitelem hello může se zobrazit chyba certifikátu v prohlížeči hello. Používá certifikát podepsaný důvěryhodnou certifikační autoritou eliminuje tento problém; v hello té doby chybu můžete ignorovat hello. (Další možností je tooadd hello certifikát podepsaný svým držitelem toohello důvěryhodné certifikační autority úložišti certifikátů uživatele.)
   > 
   > 
   
   ![SSL příklad webové stránky][3]

Pokud chcete toouse SSL pro nasazení pracovní místo produkční nasazení, je nutné nejprve toodetermine hello adresa URL použitá pro hello pracovní nasazení. Nasazení pracovní prostředí cloudové služby toohello bez zahrnutí certifikát nebo všech informací o certifikátu. Po nasazení můžete určit hello na základě GUID adresu URL, která je uvedena v hello portál Azure classic na **adresa URL webu** pole. Vytvoření certifikátu s hello běžný název (CN) rovna toohello na základě GUID adresou URL (například **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**). Použití hello Azure classic portálu tooadd hello certifikát tooyour připravený cloudové služby. Pak přidejte hello informace tooyour CSDEF a CSCFG soubory certifikátů, znovu zabalte aplikaci a aktualizovat vaše dvoufázové instalace toouse hello nový balíček pro nasazení.

## <a name="next-steps"></a>Další kroky
* [Obecná konfigurace cloudové služby](cloud-services-how-to-configure.md).
* Zjistěte, jak příliš[nasazení cloudové služby](cloud-services-how-to-create-deploy.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).
* [Správa služby cloud](cloud-services-how-to-manage.md).

[Azure classic portal]: http://manage.windowsazure.com
[0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
[1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
[2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
[3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
[4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
