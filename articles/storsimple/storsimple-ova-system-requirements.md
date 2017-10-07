---
title: "požadavky na systém aaaMicrosoft virtuální pole Azure StorSimple | Microsoft Docs"
description: "Další informace o požadavcích hello softwaru a sítě pro vaše pole virtuální zařízení StorSimple"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: ea1d3bca-e71b-453d-aa82-440d2638f5e3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2017
ms.author: alkohli
ms.openlocfilehash: 7a124873fdd806d409c7279851456e6347e7ec0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-system-requirements"></a>Požadavky systému virtuálních polí StorSimple
## <a name="overview"></a>Přehled
Tento článek popisuje hello důležité systémové požadavky pro vaše virtuální pole Microsoft Azure StorSimple a hello úložiště klientům přístup k poli hello. Doporučujeme, abyste si prošli hello informace pečlivě před nasazením systému StorSimple a pak odkazovat zpět tooit podle potřeby během nasazení a další operace.

Hello systémové požadavky:

* **Požadavky na software pro klienty úložiště** -popisuje hello podporované virtualizačních platforem, webové prohlížeče, iniciátory iSCSI, klienti protokolu SMB, požadavky na minimální virtuální zařízení a veškeré další požadavky pro tyto operační systémy.
* **Požadavky sítě pro zařízení StorSimple hello** – poskytuje informace o portech hello této toobe potřeba otevřít v tooallow vaší brány firewall pro přenosy iSCSI, cloudu nebo správu.

požadavky na systém Hello StorSimple publikované informace v tomto článku se vztahuje tooStorSimple pouze virtuální pole.

* 8000 řady zařízení přejděte příliš[požadavky na systém pro vaše zařízení řady StorSimple 8000](storsimple-system-requirements.md).
* Zařízení řady 7000 najdete příliš[požadavky na systém pro řadu zařízení StorSimple 5000 7000](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).

## <a name="software-requirements"></a>Požadavky na software
požadavky na software Hello zahrnují hello informace o hello podporované webové prohlížeče, verze protokolu SMB, virtualizačních platforem a požadavky na minimální virtuální zařízení hello.

### <a name="supported-virtualization-platforms"></a>Podporovaný virtualizační platformy
| **Hypervisor** | **Verze** |
| --- | --- |
| Hyper-V |Windows Server 2008 R2 SP1 a novější |
| VMware ESXi |5.5 a novější |

### <a name="virtual-device-requirements"></a>Požadavky na virtuální zařízení
| **Komponenta** | **Požadavek** |
| --- | --- |
| Minimální počet virtuálních procesorů (jader) |4 |
| Minimální paměť (RAM) |8 GB <br> Pro souborový server, 8 GB pro soubory menší než 2 miliony a 16 GB pro soubory 2 – 4 miliony|
| Místo na disku<sup>1</sup> |Disk s operačním systémem - 80 GB <br></br>Datový disk - 500 GB too8 TB |
| Minimální počet nejmíň jedno síťové rozhraní |1 |
| Minimální šířky pásma Internetu<sup>2</sup> |5 MB/s |

<sup>1</sup> – dynamické zřídit

<sup>2</sup> – požadavky na síť může lišit v závislosti na denní míry změny dat hello. Například pokud zařízení potřebuje tooback až 10 GB nebo další změny během dne, pak hello denní zálohování přes 5 MB/s připojení může trvat až too4.25 hodin (Pokud hello dat nelze komprimované nebo odstranění duplicit).

### <a name="supported-web-browsers"></a>Podporované webové prohlížeče
| **Komponenta** | **Verze** | **Další požadavky a poznámky** |
| --- | --- | --- |
| Microsoft Edge |nejnovější verzi | |
| Internet Explorer |nejnovější verzi |Testování s aplikací Internet Explorer 11 |
| Google Chrome |nejnovější verzi |Testovány s Chrome 46 |

### <a name="supported-storage-clients"></a>Klienti podporované úložiště
Hello následující požadavky na software se pro iniciátory iSCSI hello přístup pole virtuální zařízení StorSimple (nakonfigurovaný jako server se službou iSCSI).

| **Podporované operační systémy** | **Požadovaná verze** | **Další požadavky a poznámky** |
| --- | --- | --- |
| Windows Server |2008 R2 SP1, 2012, 2012 R2 |StorSimple můžete vytvořit dynamicky zajištěné a zcela zřizované svazky. Ji nelze vytvářet částečně zřizované svazky. Svazky zařízení StorSimple iSCSI jsou podporovány pouze pro: <ul><li>Jednoduché svazky na základní disky systému Windows.</li><li>Windows: pro formátování svazku systému souborů NTFS.</li> |

Hello následující požadavky na software jsou pro hello klientů protokolu SMB využívajících pole virtuální zařízení StorSimple (nakonfigurovaný jako souborový server).

| **Verze protokolu SMB** |
| --- |
| SMB 2.x |
| SMB 3.0 |
| SMB 3.02 |

> [!IMPORTANT]
> Zkopírujte nebo ukládat soubory chráněné službou Windows systém souborů (Encrypting File System) toohello pole virtuální zařízení StorSimple souborový server; Tato akce způsobí nepodporované konfigurace. 
> 

### <a name="supported-storage-format"></a>Podporované formát úložiště
Je podporován pouze hello úložiště objektů blob Azure bloku. Objekty BLOB stránky nejsou podporovány. Další informace [o objekty BLOB bloku a objekty BLOB stránky](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).

## <a name="networking-requirements"></a>Požadavky sítě
Hello následující tabulka uvádí hello portů, které musí toobe otevřít v tooallow vaší brány firewall pro iSCSI, SMB, cloudu nebo provoz správy. V této tabulce *v* nebo *příchozí* odkazuje toohello směr, ve kterém příchozí žádosti klientů, přístup k zařízení. *Out* nebo *odchozí* odkazuje toohello směr, ve kterém zařízení StorSimple odešle data externě, kromě nasazení hello: například odchozí toohello Internetu.

| **Číslo portu<sup>1</sup>** | **Příchozí nebo odchozí** | **Rozsah portů** | **Požadované** | **Poznámky k** |
| --- | --- | --- | --- | --- |
| TCP 80 (HTTP) |na více systémů |WAN |Ne |Odchozí port je používán pro aktualizace tooretrieve přístup k Internetu. <br></br>odchozí webový proxy server Hello je konfigurovatelná uživatelem. |
| TCP 443 (HTTPS) |na více systémů |WAN |Ano |Odchozí port se používá pro přístup k datům v cloudu hello. <br></br>odchozí webový proxy server Hello je konfigurovatelná uživatelem. |
| UDP 53 (DNS) |na více systémů |WAN |V některých případech; v části poznámky. |Tento port je povinný, jenom v případě, že používáte adresu serveru DNS pro internetový. <br></br> Všimněte si, že pokud nasazení souborového serveru, doporučujeme používat místní server DNS. |
| UDP 123 (NTP) |na více systémů |WAN |V některých případech; v části poznámky. |Tento port je povinný, jenom v případě, že používáte server NTP založené na Internetu.<br></br> Všimněte si, že pokud nasazení souborového serveru, doporučujeme, abyste synchronizaci času se řadiče domény služby Active Directory. |
| TCP 80 (HTTP) |V |LAN |Ano |To je hello portu pro příchozí spojení pro místní uživatelské rozhraní na hello zařízení StorSimple pro místní správu. <br></br> Všimněte si, že přístup k hello místního uživatelského rozhraní pomocí protokolu HTTP se automaticky přesměruje tooHTTPS. |
| TCP 443 (HTTPS) |V |LAN |Ano |To je hello portu pro příchozí spojení pro místní uživatelské rozhraní na hello zařízení StorSimple pro místní správu. |
| TCP 3260 (iSCSI) |V |LAN |Ne |Tento port je použité tooaccess data přes iSCSI. |

<sup>1</sup> musí toobe otevřít na žádné příchozí porty hello veřejného Internetu.

> [!IMPORTANT]
> Zajistěte, aby brána firewall hello upravit nebo dešifrovat veškerou komunikaci SSL mezi hello zařízení StorSimple a Azure.
> 
> 

### <a name="url-patterns-for-firewall-rules"></a>Adresa URL vzory pro pravidla brány firewall
Správci sítě můžete nakonfigurovat často rozšířené brány firewall pravidla založená na hello URL vzory toofilter hello příchozí a odchozí přenosy hello. Virtuální pole a hello služby StorSimple Manager zařízení závisí na jiných aplikací společnosti Microsoft, například Azure Service Bus, Azure Active Directory řízení přístupu, účty úložiště a servery Microsoft Update. vzorů adresy URL Hello související s těmito aplikacemi se dá použít tooconfigure pravidla brány firewall. Je důležité toounderstand, který můžete změnit vzorů adresy URL hello související s těmito aplikacemi. To zase vyžadují toomonitor správce sítě hello a aktualizovat pravidla brány firewall pro vaše zařízení StorSimple jako a v případě potřeby. 

Doporučujeme vám, že nastavíte vašich pravidlech brány firewall pro odchozí přenosy, založené na StorSimple liberally pevné IP adresy, ve většině případů. Ale můžete použít informace hello níže tooset advanced pravidla brány firewall, které jsou potřebné toocreate zabezpečené prostředí.

> [!NOTE]
> 
> * zařízení Hello (zdroj) IP adresy musí být vždy nastavená tooall hello povolenou podporu cloudu síťových rozhraní. 
> * Hello cílové IP adresy by mělo být nastavené příliš[rozsahy IP adres Azure datacenter](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).
> 
> 

| Vzor adresy URL | Komponenta nebo funkce |
| --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*` |Služba StorSimple Manager zařízení<br>Access Control Service<br>Azure Service Bus |
| `http://*.backup.windowsazure.com` |Registrace zařízení |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Odvolání certifikátu |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Monitorování a účty úložiště Azure |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |Servery Microsoft Update<br> |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |
| `https://*.partners.extranet.microsoft.com/*` |Balíček pro podporu |
| `http://*.data.microsoft.com ` |Telemetrie služby v systému Windows, najdete v části hello [aktualizace pro zkušeností zákazníků a diagnostiky telemetrii](https://support.microsoft.com/en-us/kb/3068708) |

## <a name="next-step"></a>Další krok
* [Příprava portálu toodeploy hello pole virtuální zařízení StorSimple](storsimple-virtual-array-deploy1-portal-prep.md)

