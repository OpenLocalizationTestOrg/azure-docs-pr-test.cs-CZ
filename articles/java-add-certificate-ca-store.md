---
title: "aaaAdd úložiště certifikátů certifikační Autority Java toohello | Microsoft Docs"
description: "Zjistěte, jak tooadd certifikát certifikační autority (CA) certifikát toohello Java Certifikační autority (cacerts) úložiště pro službu Twilio nebo Azure Service Bus."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: d3699b0a-835c-43fb-844d-9c25344e5cda
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 030e43129580023942dee662e72d2f443167f308
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="adding-a-certificate-toohello-java-ca-certificates-store"></a>Přidání certifikátu toohello úložiště certifikátů certifikační Autority Java
Hello následující kroky vám ukážou, jak tooadd certifikát certifikační autority (CA) certifikát toohello Java Certifikační autority (cacerts) úložiště. Příklad Hello používá pro certifikát certifikační Autority hello vyžadují hello Twilio služby. Informace uvedené dále v tématu hello popisuje, jak tooinstall hello certifikační Autority certifikátu pro hello Azure Service Bus. 

Můžete použít keytool tooadd hello certifikační Autority certifikátu předchozí toozipping vaše JDK a její přidání tooyour projektu Azure na **approot** složky, nebo může spustit úkol aplikace Azure spuštění, který používá keytool tooadd hello certifikát. Tento příklad předpokládá, že přidáte certifikační Autority certifikátu předchozí toohello JDK se metoda ZIP. Navíc konkrétní certifikát certifikační Autority se použije v hello příklad, ale hello kroky k získání jiný certifikát certifikační Autority a jeho import do hello cacerts úložiště by podobné.

## <a name="tooadd-a-certificate-toohello-cacerts-store"></a>ukládání certifikát toohello cacerts tooadd
1. Na příkazovém řádku, který je nastavený tooyour JDK **jdk\jre\lib\security** spusťte hello toosee, jaké certifikáty jsou nainstalovány následující:
   
    `keytool -list -keystore cacerts`
   
    Budete vyzváni k hello úložiště hesla. výchozí heslo Hello je **changeit**. (Pokud chcete toochange hello heslo, naleznete v dokumentaci k příkazu keytool hello v <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) Tento příklad předpokládá, že hello certifikát s MD5 otisk prstu 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 není uvedena v seznamu a mají tooimport it (Tato konkrétní certifikát je potřeba hello Twilio rozhraní API služby).
2. Získání certifikátu hello z hello seznam certifikátů, které jsou uvedeny v [GeoTrust kořenové certifikáty](http://www.geotrust.com/resources/root-certificates/). Klikněte pravým tlačítkem na odkaz hello hello certifikát se sériovým číslem 35:DE:F4:CF a uložte ho toohello **jdk\jre\lib\security** složky. Pro účely tohoto příkladu se uloží tooa soubor s názvem **Equifax\_zabezpečeného\_certifikát\_Authority.cer**.
3. Importujte certifikát hello prostřednictvím hello následující příkaz:
   
    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`
   
    Při zobrazení výzvy tootrust tohoto certifikátu, pokud se certifikát hello MD5 otisk prstu 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 reakce zadáním **y**.
4. Spuštění hello následující příkaz tooensure hello certifikační Autority certifikátu musí být úspěšně naimportována:
   
    `keytool -list -keystore cacerts`
5. Hello JDK ZIP a přidejte ji tooyour projektu Azure na **approot** složky.

Informace o příkazu keytool najdete v tématu <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Azure kořenové certifikáty
Aplikace, které používají služby Azure (například Azure Service Bus) potřebovat certifikát Baltimore CyberTrust Root tootrust hello. (Od 15. dubna 2013, Azure začal migrace z hello GTE CyberTrust Root globální toohello Baltimore CyberTrust Root. Tato migrace trvalo toocomplete několik měsíců).

Hello Baltimore certifikát již je nainstalován ve vašem úložišti cacerts proto nezapomeňte toorun hello **keytool-seznamu** příkaz první toosee, pokud již existuje.

Pokud potřebujete tooadd hello Baltimore CyberTrust Root, má 02:00:00:b9 sériové číslo a SHA1 otisk prstu d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Ho můžete stáhnout z <https://cacert.omniroot.com/bc2025.crt>, uložili tooa místní soubor s příponou **.cer**a poté importovat pomocí **keytool** jako v příkladu nahoře.

## <a name="next-steps"></a>Další kroky
Další informace o hello kořenové certifikáty používají v Azure najdete v tématu [Azure kořenový certifikát migrace](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Další informace o Java najdete v tématu [Azure pro vývojáře v jazyce Java](/java/azure).

