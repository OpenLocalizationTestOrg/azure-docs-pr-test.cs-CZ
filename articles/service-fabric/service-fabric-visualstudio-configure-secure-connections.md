---
title: "aaaConfigure zabezpečení Azure Service Fabric připojení clusteru | Microsoft Docs"
description: "Zjistěte, jak toouse Visual Studio tooconfigure zabezpečené připojení, která podporují Azure Service Fabric clusterem hello."
services: service-fabric
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: tglee
ms.assetid: 80501867-dd7a-4648-8bd6-d4f26b68402d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/04/2017
ms.author: cawa
ms.openlocfilehash: ed3e5043264cf026f74e24ca09b40ccc70086cf2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-connections-tooa-service-fabric-cluster-from-visual-studio"></a>Konfigurace clusteru Service Fabric tooa zabezpečené připojení ze sady Visual Studio
Zjistěte, jak toouse Visual Studio toosecurely přistupovat k clusteru služby Azure Service Fabric s nakonfigurované zásady řízení přístupu.

## <a name="cluster-connection-types"></a>Typy připojení clusteru
Cluster Azure Service Fabric hello podporuje dva typy připojení: **nezabezpečené** připojení a **x509 založené na certifikátech** zabezpečené připojení. (Pro clustery infrastruktury služby hostované na místních počítačích **Windows** a **dSTS** ověřování jsou také podporovány.) Typ připojení clusteru hello tooconfigure máte, když vytváří se hello cluster. Jakmile je vytvořen, nelze změnit typ připojení hello.

Nástroje sady Visual Studio Service Fabric Hello podporovat všechny typy ověřování pro připojování tooa cluster pro publikování. Najdete v části [nastavení cluster Service Fabric z portálu Azure hello](service-fabric-cluster-creation-via-portal.md) pokyny tooset zabezpečení clusteru Service Fabric.

## <a name="configure-cluster-connections-in-publish-profiles"></a>Konfigurace připojení clusteru v publikační profily
Pokud publikujete projektu Service Fabric v sadě Visual Studio, použijte hello **publikovat aplikace Service Fabric** dialogové okno pole toochoose clusteru služby Azure Service Fabric. V části **koncového bodu připojení**, vyberte existující cluster v rámci svého předplatného.

![Hello ** publikování Service Fabric aplikace ** dialogové okno je použité tooconfigure Service Fabric připojení.][publishdialog]

Hello **publikovat aplikace Service Fabric** dialogové okno automaticky ověří připojení clusteru hello. Pokud budete vyzváni, přihlaste se tooyour účet Azure. V případě úspěšného ověření, znamená to, že má váš systém správné nainstalované certifikáty tooconnect toohello clusteru bezpečně nebo clusteru je nezabezpečené hello. Selhání ověření můžete způsobeno problémy se síťovým nebo nemá clusteru zabezpečené tooa tooconnect systému správně nakonfigurovaný.

![Hello ** publikování Service Fabric aplikace ** dialogové okno ověří stávající správně nakonfigurované připojení clusteru Service Fabric.][selectsfcluster]

### <a name="tooconnect-tooa-secure-cluster"></a>tooconnect tooa zabezpečení clusteru
1. Ujistěte se, že dostanete mezi hello klientské certifikáty, které hello vztahy důvěryhodnosti cílového clusteru. certifikát Hello se obvykle sdílí jako soubor Personal Information Exchange (.pfx). V tématu [nastavení cluster Service Fabric z portálu Azure hello](service-fabric-cluster-creation-via-portal.md) pro přístupu tooconfigure hello server toogrant tooa klienta.
2. Nainstalujte certifikát důvěryhodné hello. toodo, poklikejte na soubor .pfx hello, nebo použijte hello prostředí PowerShell skriptu importu PfxCertificate tooimport hello certifikáty. Nainstalujte certifikát hello příliš**Cert: \LocalMachine\My**. Je OK tooaccept všechna výchozí nastavení při importu certifikátu hello.
3. Zvolte hello **publikování...**  příkaz v místní nabídce hello hello projektu tooopen hello **publikování aplikaci Azure** dialogové okno a potom vyberte hello cílový cluster. Nástroj Hello automaticky vyřeší hello připojení a uloží hello zabezpečené připojení, které parametry v hello Publikovat profil.
4. Volitelné: Můžete upravit hello publikování toospecify profil připojení zabezpečení clusteru.
   
   Vzhledem k tomu, že ručně upravujete hello Publikovat profil XML soubor toospecify hello informací o certifikátu, že název úložiště certifikátu toonote hello, uložení umístění a kryptografický otisk certifikátu. Budete potřebovat tooprovide tyto hodnoty pro úložiště certifikátů hello název a umístění úložiště. V tématu [postupy: načtení hello kryptografického otisku certifikátu](https://msdn.microsoft.com/library/ms734695\(v=vs.110\).aspx) Další informace.
   
   Můžete použít hello *ClusterConnectionParameters* parametry toospecify hello prostředí PowerShell parametry toouse při připojování toohello cluster Service Fabric. Platné parametry jsou všechny, které přijímají hello Connect-ServiceFabricCluster rutinou. V tématu [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) seznam dostupných parametrů.
   
   Pokud publikujete tooa vzdálený cluster, musíte pro tento konkrétní cluster toospecify hello příslušné parametry. Hello tady je příklad připojování tooa nezabezpečené clusteru:
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   Tady je příklad pro připojování tooan x509, na základě certifikátu zabezpečeného clusteru:
   
   ```xml
   <ClusterConnectionParameters
   ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000"
   X509Credential="true"
   ServerCertThumbprint="0123456789012345678901234567890123456789"
   FindType="FindByThumbprint"
   FindValue="9876543210987654321098765432109876543210"
   StoreLocation="CurrentUser"
   StoreName="My" />
   ```
5. Upravit další potřebné nastavení, například upgradu parametry a umístění souboru aplikace parametr a pak publikujte aplikaci z hello **publikovat aplikace Service Fabric** dialogové okno v sadě Visual Studio.

## <a name="next-steps"></a>Další kroky
Další informace o přístupu k clusterů Service Fabric najdete v tématu [vizualizace vašeho clusteru pomocí Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
