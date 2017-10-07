---
title: "aaaUsing certifikáty s Enterprise integračního balíčku | Microsoft Docs"
description: "Zjistěte, jak toouse certifikáty s hello Enterprise integračního balíčku | Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: cgronlun
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 7ba9f597a03a852a9ba05d2af08fe4bc2d98aef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Další informace o certifikátech a řešení Enterprise Integration Pack
## <a name="overview"></a>Přehled
Integrace Enterprise používá komunikace B2B toosecure certifikáty. Můžete provádět dva typy certifikátů ve vaší podnikové integrace aplikace:

* Veřejné certifikáty, které je třeba zakoupit od certifikační autority (CA).
* Privátní certifikáty, které můžete zadat sami. Tyto certifikáty jsou někdy označují tooas certifikáty podepsané svým držitelem.

## <a name="what-are-certificates"></a>Co jsou certifikáty?
Certifikáty jsou digitální dokumenty, ověřit, zda text hello identita hello účastníky v elektronické komunikace a která taky zabezpečené elektronické komunikace.

## <a name="why-use-certificates"></a>Proč používat certifikáty?
Někdy komunikace B2B musí být důvěrné. Integrace Enterprise používá certifikáty toosecure tyto komunikace dvěma způsoby:

* Šifrování hello obsah zprávy
* Podle digitálnímu podepisování zpráv  

## <a name="upload-a-public-certificate"></a>Nahrát veřejného certifikátu

toouse *veřejný certifikát* ve vašich logic apps možnosti B2B, musíte nejprve tooupload hello certifikátu do účtu integrace.  

Po odeslání certifikát, je k dispozici toohelp zabezpečení zpráv B2B při definování jejich vlastnosti v hello [smlouvy](logic-apps-enterprise-integration-agreements.md) , kterou vytvoříte.  

Tady jsou hello podrobné kroky pro nahrávání veřejné certifikáty do účtu integrace po přihlášení toohello portálu Azure:

1. Vyberte **další služby** a zadejte **integrace** hello filtru vyhledávacího pole. Vyberte **účty pro integraci** ze seznamu výsledků hello     
![Vyberte procházení](media/logic-apps-enterprise-integration-certificates/overview-1.png)  
2. Vyberte hello integrace účet toowhich chcete tooadd hello certifikátu.  
![Vyberte hello integrace účet toowhich chcete tooadd hello certifikátu](media/logic-apps-enterprise-integration-certificates/overview-3.png)  
3. Vyberte hello **certifikáty** dlaždici.  
![Dlaždice certifikáty vyberte hello](media/logic-apps-enterprise-integration-certificates/certificate-1.png)
4. V hello **certifikáty** okno, které se otevře, vyberte hello **přidat** tlačítko.   
![Kliknutím na tlačítko Přidat hello](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
5. Zadejte **název** certifikát, a pak vyberte hello typ certifikátu jako **veřejné** z rozevíracího seznamu hello.  
6. Vyberte hello ikonou složky na pravé straně hello hello **certifikát** textové pole. Když se otevře okno pro výběr souboru hello, najděte a vyberte soubor certifikátu hello, které chcete tooupload tooyour integrace účtu.
7. Vyberte certifikát hello a pak vyberte **OK** v hello pro výběr souborů. To ověří a odesílá je účet integrace tooyour hello certifikátu.
8. Nakonec zpět na hello **přidat certifikát** okně, vyberte hello **OK** tlačítko.  
![Kliknutím na tlačítko OK hello](media/logic-apps-enterprise-integration-certificates/certificate-3.png)  
9. Vyberte hello **certifikáty** dlaždici. Měli byste vidět, že hello nově přidaná certifikátu.  
![V tématu hello nový certifikát](media/logic-apps-enterprise-integration-certificates/certificate-4.png)  

## <a name="upload-a-private-certificate"></a>Nahrát privátní certifikát

toouse *privátní certifikát* ve vašich logic apps s B2B možnosti, můžete nahrát účet privátní certifikát tooyour integrace podle trvá hello následující kroky

1. [Nahrát vaší privátní klíče tooKey trezoru](../key-vault/key-vault-get-started.md "Další informace o Key Vault") a poskytují **název klíče** 
   
   > [!TIP]
   > Je nutné autorizovat Logic Apps tooperform operace v Key Vault. Můžete udělit přístup toohello Logic Apps instanční objekt pomocí hello následující příkaz prostředí PowerShell:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  
   > 
   > 

Poté, co jste prováděné hello předchozím kroku, přidáte účet toointegration privátní certifikát.

Podrobné kroky pro nahrávání privátní certifikáty do účtu integrace po přihlášení toohello portálu Azure jsou hello následující:  
 
1. Vyberte hello integrace účet toowhich tooadd hello certifikátu a zaškrtněte hello **certifikáty** dlaždici.  
![Dlaždice certifikáty vyberte hello](media/logic-apps-enterprise-integration-certificates/certificate-1.png)  
2. V hello **certifikáty** okno, které se otevře, vyberte hello **přidat** tlačítko.   
![Kliknutím na tlačítko Přidat hello](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
3. Zadejte **název** certifikát, a typ certifikátu vyberte hello jako **privátní** z rozevíracího seznamu hello.   
4. Vyberte ikonu složky hello na pravé straně hello hello **certifikát** textové pole. Když se otevře okno pro výběr souboru hello, najdete hello odpovídající veřejný certifikát, který má tooupload tooyour integrace účtu.   
   
   > [!Note]
   > Při přidávání privátního certifikátu je důležité tooadd odpovídající veřejný certifikát tooshow v [smlouvy AS2](logic-apps-enterprise-integration-as2.md) příjem a odesílání nastavení pro podepisování a šifrování zpráv hello.
   > 
   >   

5. Vyberte hello **skupiny prostředků**, **Key Vault**, **název klíče** a vyberte hello **OK** tlačítko.  
![Přidání certifikátu](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)  
6. Vyberte hello **certifikáty** dlaždici. Měli byste vidět, že hello nově přidaná certifikátu.
![V tématu hello nový certifikát](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)  



* [Vytvoření smlouvy s B2B](logic-apps-enterprise-integration-agreements.md)  
* [Další informace o Key Vault](../key-vault/key-vault-get-started.md "Další informace o Key Vault")  

