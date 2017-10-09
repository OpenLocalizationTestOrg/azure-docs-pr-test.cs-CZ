---
title: "aaaSet se zásada replikace pro tooa replikace technologie Hyper-V s Azure Site Recovery sekundární lokality. | Microsoft Docs"
description: "Popisuje, jak tooset si zásadu pro virtuální počítač Hyper-V replikace tooa VMM sekundární lokalitu s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5d9b79cf-89f2-4af9-ac8e-3a32ad8c6c4d
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 6d008e3bb3fa0b666e91678cf6de3693dd712ae3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-a-replication-policy"></a>Krok 8: Nastavení zásad replikace

Po dokončení konfigurace [mapování sítě](vmm-to-vmm-walkthrough-network-mapping.md), použijte tento článek tooset si zásadu replikace pro Hyper-V virtuální počítač (VM) replikace tooa sekundární lokalitu pomocí [Azure Site Recovery](site-recovery-overview.md).

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Než začnete

- Když vytvoříte zásadu replikace, všechny hostitele pomocí zásad hello musí mít hello stejný operační systém. Hello cloudu VMM může obsahovat hostitelů Hyper-V s různými verzemi systému Windows Server, ale v takovém případě musíte víc zásad replikace.
- Můžete provést hello počáteční replikaci v režimu offline.

## <a name="configure-replication-settings"></a>Konfigurace nastavení replikace

1. Klikněte na tlačítko toocreate novou zásadu replikace, **připravit infrastrukturu** > **nastavení replikace** > **+ vytvořit a přidružit**.

    ![Síť](./media/vmm-to-vmm-walkthrough-replication/gs-replication.png)
2. V části **Vytvořit a přidružit zásady** zadejte název zásady. Hello zdrojové a cílové typ musí být **technologie Hyper-V**.
3. V **verze hostitele technologie Hyper-V**, vyberte, jaký operační systém běží na hostiteli hello.
4. V **typ ověřování** a **port ověřování**, zadejte, jak ověření přenosů mezi hello primární a servery hostitele obnovení technologie Hyper-V. Vyberte **certifikát** pouze tehdy, je pracovní prostředí protokolu Kerberos. Azure Site Recovery automaticky nakonfiguruje certifikáty pro ověřování pomocí protokolu HTTPS. Nepotřebujete toodo nic ručně. Ve výchozím nastavení port 8083 a 8084 (pro certifikáty) se otevřou v hello brány Windows Firewall na hostitelských serverech technologie Hyper-V hello. Pokud vyberete **Kerberos**, lístek protokolu Kerberos se použije pro vzájemné ověřování serverů hostitele hello. Všimněte si, že toto nastavení platí jen pro hostitelské servery technologie Hyper-V v systému Windows Server 2012 R2.
5. V **frekvence kopírování**, určete, jak často má tooreplicate rozdílová data po hello počáteční replikaci (každých 30 sekund, 5 nebo 15 minut).
6. V **uchování bodu obnovení**, zadejte v hodinách, jak dlouho bude hello intervalem pro uchovávání dat se pro každý bod obnovení. Chráněné počítače může být obnovena tooany bodu v rámci časového období.
7. V **frekvence snímkování konzistentní aplikace vzhledem**, zadejte, jak často (1 – 12 hodin) body obnovení obsahující snímky konzistentní s aplikacemi jsou vytvořeny. Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače hello a snímky konzistentní s aplikací, která přebírá v okamžiku snímek dat aplikací hello uvnitř hello virtuálního počítače. Snímky konzistentní s aplikacemi pomocí tooensure služby Stínová kopie svazku (VSS), které aplikace budou při pořízení snímku hello v konzistentním stavu. Pokud povolíte snímky konzistentní s aplikacemi, ovlivní hello výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byl hello hodnoty, které nastavíte, menší než počet hello další body obnovení, které nakonfigurujete.
8. V **kompresi přenosu dat**, zadejte, zda by měl být komprimované replikovaná data, která se přenáší.
9. Vyberte **odstranit repliku virtuálního počítače**, měla by být odstraněna toospecify, který hello virtuální počítač repliky, pokud zakažte ochranu pro hello zdrojového virtuálního počítače. Pokud povolíte toto nastavení při zakázání ochrany pro hello zdrojového virtuálního počítače se odebere z konzole Site Recovery hello, odeberou se z konzoly VMM hello nastavení Site Recovery pro hello VMM a hello repliky se odstraní.
10. V **počáteční metodu replikace**, pokud replikujete hello síti, určete, zda text hello počáteční replikace toostart, nebo ji můžete naplánovat. toosave šířku pásma sítě, můžete chtít tooschedule ho dobu mimo špičky. Pak klikněte na **OK**.

     ![Zásady replikace](./media/vmm-to-vmm-walkthrough-replication/gs-replication2.png)
11. Když vytvoříte novou zásadu je přidružená automaticky hello cloudu VMM. V **zásady replikace**, klikněte na tlačítko **OK**. K této zásadě replikace můžete přidružit další Cloudy VMM (a hello virtuální počítače v nich) **replikace** > název zásady > **přidružit Cloud VMM**.

     ![Zásady replikace](./media/vmm-to-vmm-walkthrough-replication/policy-associate.png)



## <a name="prepare-for-offline-initial-replication"></a>Příprava pro počáteční replikaci prováděnou offline

Můžete provést offline replikaci pro kopírování dat počáteční hello. Můžete to následujícím způsobem připravit:

* Na zdrojovém serveru hello zadejte cestu umístění, ze které hello bude export dat probíhat. Přiřaďte úplné řízení pro oprávnění systému souborů NTFS a sdílení toohello služba VMM na cestu pro export hello. Na cílovém serveru hello určíte, že dojde k cestu umístění, ze kterého hello data importovat. Přiřaďte hello stejná oprávnění v této cestě importu.
* Pokud hello import nebo export cesta sdílená, přiřadit správce, Power Users, Print Operator nebo operátor serveru členství ve skupině pro účet služby VMM hello ve vzdáleném počítači hello na které hello sdílet se nachází.
* Pokud používáte všechny spustit jako účty tooadd hostitele, hello importovat a exportovat cesty, přiřadit pro čtení a zápis toohello účty spustit jako v nástroji VMM.
* Hello import a export sdílených složek nesmí nacházet na libovolném počítači se používá jako server hostitele technologie Hyper-V, protože konfigurace zpětné smyčky není podporována technologií Hyper-V.
* Ve službě Active Directory, na každém serveru hostitele technologie Hyper-V, který obsahuje virtuální počítače, které chcete tooprotect povolte a nakonfigurujte omezené delegování tootrust hello vzdálené počítačích, na které hello import a export cesty jsou umístěny, následujícím způsobem:
  1. V řadiči domény hello otevřete **Active Directory Users and Computers**.
  2. Ve stromu konzoly hello, klikněte na tlačítko **DomainName** > **počítače**.
  3. Název hostitelského serveru klikněte pravým tlačítkem na hello technologie Hyper-V > **vlastnosti**.
  4. Na hello **delegování** , klikněte na **důvěřovat tomuto počítači pro delegování pouze toospecified službám**.
  5. Klikněte na tlačítko **používající libovolný protokol pro ověřování**.
  6. Klikněte na tlačítko **přidat** > **uživatelé a počítače**.
  7. Název typu hello hello počítače, který je hostitelem cestu pro export hello > **OK**. Hello seznamu dostupných služeb, podržte klávesu CTRL hello a klikněte na **cifs** > **OK**. Opakujte pro hello název počítače hello cestu importu hello tohoto hostitele. Opakujte podle potřeby pro další hostitelské servery technologie Hyper-V.



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 9: povolení replikace](vmm-to-vmm-walkthrough-enable-replication.md).
