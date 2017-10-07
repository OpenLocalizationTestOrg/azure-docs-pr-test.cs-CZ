---
title: "Poznámky k verzi aaaAzure katalogu Data Catalog | Microsoft Docs"
description: "Poznámky k verzi pro Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 3aca9c49-45a4-4352-92e6-bd25ee3eacf7
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 661826f66020ba72a885c6b14522b53c8b469d20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-release-notes"></a>Poznámky k verzi služby Azure Data Catalog
## <a name="notes-for-hello-november-20-2015-release-of-azure-data-catalog"></a>Poznámky k verzi 20. listopadu 2015 hello služby Azure Data Catalog
### <a name="opening-data-sources-in-power-bi-desktop"></a>Otevírání zdroje dat v Power BI Desktop
Při použití možnosti "Otevřít v Power BI Desktop" hello z hello **Azure Data Catalog** portálu, uživatelé setkat s jedním z dva problémy v aplikaci Power BI Desktop hello:

* Zobrazí se dialogové okno s názvem hello "Nelze tooOpen dokumentu"
* Otevře Hello aplikace Power BI Desktop, ale soubor hello se zobrazí toobe prázdný

Pro každé situaci hello problému stahuje a instaluje hello nejnovější verzi aplikace Power BI Desktop z [PowerBI.com](https://powerbi.com).

## <a name="notes-for-hello-november-13-2015-release-of-azure-data-catalog"></a>Poznámky k verzi 13. listopadu 2015 hello služby Azure Data Catalog
### <a name="registering-and-connecting-tooteradata"></a>Registrace a připojení tooTeradata
Při připojování tooTeradata zdroje dat uživatelům musí mít nainstalované správné ovladače Teradata ODBC hello odpovídající hello počtu bitů (32bitová nebo 64bitová verze) hello softwaru používán.

Od této ADC datum vydání, hello nejnovější [ovladač Teradata ODBC pro windows (verze 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) je kompatibilní s Office 2013, ale ne při Office 2016.

## <a name="notes-for-hello-july-13-2015-release-of-azure-data-catalog"></a>Poznámky k verzi 13. července 2015 hello služby Azure Data Catalog
### <a name="registering-and-connecting-toooracle-database"></a>Registrace a připojení tooOracle databáze
Při připojování uživatelů zdroje dat databáze tooOracle mají nainstalovánu hello správné Oracle ovladače, které odpovídají hello počtu bitů (32bitová nebo 64bitová verze) hello softwaru používán.

* Při registraci zdroje dat Oracle na počítači se systémem Windows 32-bit, použije se hello 32bitové Oracle ovladače
* Při registraci zdroje dat Oracle na počítači se systémem Windows 64-bit, použije se hello 64-bit Oracle ovladače
* Při připojování tooOracle zdroje dat pomocí aplikace Excel v počítači se systémem hello 32bitovou verzi systému Microsoft Office, včetně na 64bitovém systému Windows, bude použit hello 32bitové Oracle ovladače
* Při připojování tooOracle zdroje dat pomocí aplikace Excel v počítači se systémem hello 64bitová verze systému Microsoft Office, použije se hello 64-bit Oracle ovladače

### <a name="registering-and-connecting-toosql-server-reporting-services"></a>Registrace a připojení tooSQL Server Reporting Services
Podpora pro zdroje dat služby SQL Server Reporting Services (SSRS) je aktuálně omezená tooNative režimu pouze servery. Podpora služby SSRS v režimu serveru SharePoint přidá v novější verzi.

### <a name="opening-data-assets-in-excel"></a>Otevírání datových prostředků v aplikaci Excel
Při otevření datové prostředky v aplikaci Microsoft Excel z hello **Azure Data Catalog** portálu, může se uživatelům výzva s **oznámení o zabezpečení aplikace Microsoft Excel** dialogové okno. Toto je standardní, můžete vybrat očekávané chování a uživatelé **povolit** toocontinue.

Další informace najdete v tématu [povolit nebo zakázat výstrahy zabezpečení týkající se propojení a souborů z podezřelé weby](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Proxy server, zásad konfigurace a data registrace zdroje
Uživatelé mohou nastat situace, kdy může přihlásit na portál Azure Data Catalog toohello, ale když se pokoušejí toolog na toohello nástroj registrace zdroje dat narazí chybovou zprávu, která brání protokolování.

Existují dvě možné příčiny tohoto problému chování:

**Příčina 1: Konfigurace služby Active Directory Federation Services** nástroj registrace zdroje dat hello používá ověřování pomocí formulářů toovalidate přihlášení uživatelů pro službu Active Directory. Pro úspěšné přihlášení musí být povolené ověřování pomocí formulářů ve hello globální zásady ověřování správcem služby Active Directory.

V některých situacích toto chování chybě může dojít pouze v případě, že uživatel hello je v síti společnosti hello nebo jenom v případě, že uživatel hello připojuje z mimo hello podnikové síti. Hello globální zásady ověřování umožňuje toobe metody ověřování pro intranet a extranet připojení povoleno samostatně. Pokud je ověřování pomocí formulářů není povoleno pro hello síť, ze které hello se uživatel připojuje, může dojít k chybám přihlášení.

Další informace najdete v tématu [konfigurace zásad ověřování](https://technet.microsoft.com/library/dn486781.aspx).

**Příčina 2: Konfigurace proxy serveru sítě** Pokud hello podniková síť používá proxy server, nástroj pro registraci hello nemusí být možné tooconnect tooAzure služby Active Directory přes proxy server hello. Uživatele můžete zajistit tento nástroj pro registraci hello úpravou konfiguračního souboru nástroje hello, přidání tohoto souboru toohello části:

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


toolocate hello RegistrationTool.exe.config souboru, spusťte nástroj pro registraci hello a pak otevřete nástroj Správce úloh systému Windows hello. Na kartě Podrobnosti hello ve Správci úloh klikněte pravým tlačítkem na RegistrationTool.exe a hello rozbalovací nabídce vyberte příkaz Otevřít umístění souboru.
