---
title: "Služby a správu certifikátů aaaCloud | Microsoft Docs"
description: "Zjistěte, jak toocreate a používání certifikátů v Microsoft Azure"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: fc70d00d-899b-4771-855f-44574dc4bfc6
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 69cb5467ece58a91dae06b4120954aeb2826bde1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="certificates-overview-for-azure-cloud-services"></a>Přehled certifikáty pro cloudové služby Azure
Certifikáty se používají v Azure pro cloudové služby ([služby certifikáty](#what-are-service-certificates)) a pro ověřování pomocí rozhraní API pro správu hello ([certifikáty pro správu](#what-are-management-certificates) při použití hello portál Azure classic a ne Hello ne klasický portál Azure). Toto téma obsahuje obecný přehled oba typy certifikátů, jak příliš[vytvořit](#create) a [nasazení](#deploy) je tooAzure.

Certifikáty používané v Azure jsou x.509 v3 certifikáty a může být podepsány jiný certifikát důvěryhodné nebo mohou být podepsaný svým držitelem. Certifikát podepsaný svým držitelem je podepsána vlastní creator, proto není důvěryhodný ve výchozím nastavení. Tento problém můžete ignorovat většina prohlížečů. Byste měli používat jenom certifikáty podepsané svým držitelem při vývoji a testování vaší cloudové služby. 

Certifikáty používané Azure může obsahovat privátního nebo veřejného klíče. Kryptografický otisk, která poskytuje tooidentify znamená mít certifikáty je jednoznačným způsobem. Tímto kryptografickým otiskem se používá v hello Azure [konfigurační soubor](cloud-services-configure-ssl-certificate.md) tooidentify, který certifikát Cloudová služba by měl používat. 

## <a name="what-are-service-certificates"></a>Jaké jsou certifikáty služby?
Certifikáty služby jsou připojené toocloud služby a povolte tooand zabezpečenou komunikaci ze služby hello. Například pokud jste nasadili webovou roli, je vhodné toosupply certifikát, který může ověřit zveřejněné koncový bod HTTPS. Certifikáty služby definované v definice služby, jsou automaticky nasazené toohello virtuální počítač, který je spuštěna instance role. 

Můžete nahrát služby certifikáty tooAzure klasickém portálu buď pomocí hello portál Azure classic portálu nebo pomocí modelu nasazení classic hello. Certifikáty služby jsou přidruženy ke konkrétní cloudové službě. Jsou přiřazeny tooa nasazení v souboru definice služby hello.

Certifikáty služby může být spravován od něj odděleně vašim službám a mohou být spravovány nástrojem různí jednotlivci. Vývojář může například odeslat balíček služby, která odkazuje správce IT se předtím nahrála tooAzure certifikát tooa. Správce IT můžete spravovat a obnovit tento certifikát (Změna konfigurace hello služby hello) bez nutnosti tooupload nový balíček pro službu. Aktualizace bez nový balíček služby je možné, protože logický název hello, název úložiště a umístění certifikátu hello je v souboru definice služby hello a když hello kryptografický otisk certifikátu je uveden v konfiguračním souboru služby hello. tooupdate hello certifikát, je pouze nezbytné tooupload nový certifikát a kryptografický otisk hello změnit hodnotu v konfiguračním souboru služby hello.

>[!Note]
>Hello [nejčastější dotazy týkající se služby Cloud](cloud-services-faq.md) článek obsahuje některé užitečné informace o certifikátech.

## <a name="what-are-management-certificates"></a>Jaké jsou certifikáty pro správu?
Certifikáty pro správu povolit tooauthenticate s modelem nasazení classic hello. Mnoho programy a nástroje (například Visual Studio nebo hello Azure SDK) používat tyto certifikáty tooautomate konfigurace a nasazení různých služeb Azure. Tyto nejsou skutečně související toocloud služby. 

> [!WARNING]
> Dej si pozor! Tyto typy certifikátů povolit všem uživatelům, kteří s nimi ověřuje toomanage hello předplatného jsou přidruženy. 
> 
> 

### <a name="limitations"></a>Omezení
Existuje limit 100 certifikáty pro správu podle předplatného. Je také limit 100 certifikáty pro správu pro všechna předplatná pod ID specifické služby správce uživatele. Pokud hello ID uživatele pro správce účtu hello již bylo použité tooadd 100 certifikáty pro správu a je nezbytné pro další certifikáty, můžete přidat další certifikáty spolusprávcem tooadd hello. 

Před přidáním více než 100 certifikáty, najdete v části Pokud můžete opakovaně použít stávající certifikát. Pomocí spolusprávci přidá potenciálně nepotřebné složitost tooyour certifikátu správy.

<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Vytvořit nový certifikát podepsaný svým držitelem
K dispozici toocreate žádné nástroj certifikát podepsaný svým držitelem můžete použít, dokud budou vyhovovat toothese nastavení:

* Certifikát X.509.
* Obsahuje privátní klíč.
* Vytvoří pro výměnu klíčů (soubor .pfx).
* Název subjektu musí odpovídat hello domény použít tooaccess hello cloudové služby.

    > Nelze získat certifikát protokolu SSL pro hello cloudapp.net (nebo pro jakékoli souvisejících s Azure) domény; Název subjektu certifikátu Hello musí odpovídat tooaccess název používaný hello vlastní domény aplikace. Například **contoso.net**, nikoli **contoso.cloudapp.net**.

* Minimálně 2048bitové šifrování.
* **Služba certifikátu pouze**: klientský certifikát se musí nacházet v hello *osobní* úložiště certifikátů.

Existují dva způsoby, jak snadno toocreate certifikát v systému Windows, s hello `makecert.exe` nástroje nebo služby IIS.

### <a name="makecertexe"></a>MakeCert.exe
Tento nástroj je zastaralá a již jsou zde uvedeny. Další informace najdete v tématu [tohoto článku na webu MSDN](https://msdn.microsoft.com/library/windows/desktop/aa386968).

### <a name="powershell"></a>PowerShell
```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

> [!NOTE]
> Pokud chcete certifikát hello toouse s IP adresou místo k doméně, použijte parametr - DnsName hello hello IP adresu.


Pokud chcete, aby toouse to [certifikátu pomocí portálu správy hello](../azure-api-management-certs.md), ho exportovat tooa **.cer** souboru:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internetová informační služba (IIS)
Existuje mnoho stránek na hello internet, které zahrnují jak toodo to pomocí služby IIS. [Zde](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) je skvělým našel jsem myslím ji a vysvětluje. 

### <a name="java"></a>Java
Můžete použít Java příliš[vytvořit certifikát](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).

### <a name="linux"></a>Linux
[To](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) článek popisuje, jak toocreate certifikáty pomocí protokolu SSH.

## <a name="next-steps"></a>Další kroky
[Nahrajte certifikát toohello vaší služby portálu Azure classic](cloud-services-configure-ssl-certificate.md) (nebo hello [portál Azure](cloud-services-configure-ssl-certificate-portal.md)).

Nahrát [certifikát správy rozhraní API](../azure-api-management-certs.md) toohello portál Azure classic. Hello portál Azure nepoužívá certifikáty pro správu pro ověřování.

