---
title: "Příprava aaaPortal pro pole virtuální zařízení StorSimple | Microsoft Docs"
description: "První kurz toodeploy pole virtuální zařízení StorSimple spočívá v přípravě hello portálu Azure"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 68a4cfd3-94c9-46cb-805c-46217290ce02
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5332b235e7296a9274f2e7dafcdf72f4b9cdadf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---prepare-hello-azure-portal"></a>Nasazení pole virtuálního zařízení StorSimple – Příprava hello portálu Azure

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a>Přehled

Toto je první článek hello v řadě hello kurzy nasazení vyžaduje toocompletely nasadit virtuální pole jako souborový server nebo server se službou iSCSI pomocí modelu Resource Manager hello. Tento článek popisuje hello přípravy požadované toocreate a konfigurace vašeho správce zařízení StorSimple služby předchozí tooprovisioning virtuální pole. Tento článek taky obsahuje odkazy na požadavky konfigurace a kontrolní seznam nasazení konfigurace tooa.

Je třeba správce oprávnění toocomplete hello procesu instalace a konfigurace. Doporučujeme, abyste si prošli kontrolní seznam konfigurace nasazení hello před zahájením. Příprava portálu Hello trvá méně než 10 minut.

informace o Hello publikované v tomto článku se vztahuje toohello nasazení pole virtuální zařízení StorSimple v hello portál Azure a cloudu Microsoft Azure Government.

### <a name="get-started"></a>Začínáme
pracovní postup nasazení Hello se skládá z přípravy hello portál, zřizování virtuální pole ve virtualizovaném prostředí a dokončení instalace hello. tooget začít s hello pole virtuální zařízení StorSimple nasazení jako souborový server nebo server se službou iSCSI, je nutné toorefer toohello následující tabulce prostředky.

#### <a name="deployment-articles"></a>Články nasazení

toodeploy pole virtuální zařízení StorSimple, najdete v toohello následující články v pořadí určeném hello.

| **#** | **V tomto kroku** | **To uděláte...** | **A použijte tyto dokumenty.** |
| --- | --- | --- | --- |
| 1. |**Nastavit hello portálu Azure** |Vytvoření a konfigurace vašeho správce zařízení StorSimple služby předchozí tooprovisioning o pole virtuální zařízení StorSimple. |[Příprava portálu hello](storsimple-virtual-array-deploy1-portal-prep.md) |
| 2. |**Zřídit hello virtuální pole** |Pro Hyper-V zřídíte a připojit tooa pole virtuální zařízení StorSimple v systému hostitele s technologií Hyper-V v systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2. <br></br> <br></br> Pro VMware zřídíte a připojit tooa pole virtuální zařízení StorSimple v hostitelském systému, systémem VMware ESXi 5.5 a vyšší.<br></br> |[Zřídit virtuální pole Hyper-v](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [Zřídit virtuální pole v prostředí VMware](storsimple-virtual-array-deploy2-provision-vmware.md) |
| 3. |**Nastavit hello virtuální pole** |Pro souborový server provedení počáteční instalace, zaregistrujte zařízení StorSimple souborového serveru a dokončení instalace zařízení hello. Potom můžete zřídit sdílené složky protokolu SMB. <br></br> <br></br> Pro váš server iSCSI provedení počáteční instalace, zaregistrujte serveru iSCSI StorSimple a dokončení instalace zařízení hello. Potom můžete zřídit svazky iSCSI. |[Nastavit virtuální pole jako souborový server](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[Nastavit virtuální pole jako iSCSI server](storsimple-virtual-array-deploy3-iscsi-setup.md) |

Teď můžete začít tooset až hello portálu Azure.

## <a name="configuration-checklist"></a>Kontrolní seznam konfigurace

kontrolní seznam konfigurace Hello popisuje hello informace, je nutné toocollect před konfigurací softwaru hello na pole virtuální zařízení StorSimple. Příprava tyto informace před čas pomáhá zjednodušit hello proces nasazení zařízení StorSimple hello ve vašem prostředí. V závislosti na tom, jestli je vaše pole virtuální zařízení StorSimple nasazený jako souborový server nebo server se službou iSCSI, budete potřebovat hello následující kontrolní seznamy.

* Stáhnout hello [StorSimple virtuální pole souboru serveru kontrolní seznam konfigurace](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).
* Stáhnout hello [pole virtuální zařízení StorSimple iSCSI kontrolní seznam konfigurace serveru](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Požadavky

Zde zjistíte hello požadavky konfigurace služby StorSimple Manager zařízení, pole virtuální zařízení StorSimple a hello síti datového centra.

### <a name="for-hello-storsimple-device-manager-service"></a>Pro hello služby StorSimple Manager zařízení

Než začnete, ujistěte se, že:

* Máte účet Microsoft a přihlašovací údaje účtu.
* Máte účet služby Microsoft Azure Storage a přihlašovací údaje účtu.
* Vaše předplatné Microsoft Azure by měl povolit pro službu StorSimple Manager zařízení.

### <a name="for-hello-storsimple-virtual-array"></a>Pro hello pole virtuální zařízení StorSimple

Před nasazením virtuální pole, ujistěte se, že:

* Máte přístup tooa hostitelský systém s technologií Hyper-V v systému Windows Server 2008 R2 nebo novější nebo VMware (ESXi 5.5 nebo novější), které můžou být použité tooa zřízení zařízení.
* Hello systém hostitele je možné toodedicate hello následující prostředky tooprovision virtuální pole:
  
  * Minimálně 4 jádra.
  * Alespoň 8 GB paměti RAM. Pokud máte v plánu tooconfigure hello virtuální pole jako souborový server, 8 GB podporuje 2 miliony souborů. Je nutné 16 GB paměti RAM toosupport 2 – 4 miliony souborů.
  * Jedno síťové rozhraní.
  * 500 GB virtuální disk pro data systému.

### <a name="for-hello-datacenter-network"></a>Pro síť datového centra hello

Než začnete, ujistěte se, že:

* podle hello síťové požadavky pro zařízení StorSimple je nakonfigurován Hello sítě ve vašem datovém centru. Další informace najdete v tématu hello [StorSimple virtuální požadavky na systém pole](storsimple-ova-system-requirements.md).
* Pole virtuální zařízení StorSimple nemá vyhrazené 5 MB/s šířky pásma Internetu (nebo má více) vždy k dispozici. Tato šířky pásma nesmí sdílet s jinými aplikacemi.

## <a name="step-by-step-preparation"></a>Krok za krokem přípravy

Pomocí portálu hello následující tooprepare podrobné pokyny pro hello služby StorSimple Manager zařízení.

## <a name="step-1-create-a-new-service"></a>Krok 1: Vytvoření nové služby

Jedna instance služby StorSimple Manager zařízení hello můžete spravovat několik polí virtuální zařízení StorSimple. Proveďte následující kroky toocreate instanci služby StorSimple Manager zařízení hello hello. Pokud máte vaše virtuální pole stávající služby toomanage Správce zařízení StorSimple, tento krok přeskočte a přejděte příliš[krok 2: registrační klíč služby hello Get](#step-2-get-the-service-registration-key).

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> Pokud jste nepovolili automatické vytvoření účtu úložiště hello s službou, budete potřebovat toocreate alespoň jeden účet úložiště po úspěšném vytvoření služby.
> 
> * Pokud nebyl vytvořen účet úložiště automaticky, přejděte příliš[konfigurace nového účtu úložiště pro službu hello](#optional-step-configure-a-new-storage-account-for-the-service) podrobné pokyny.
> * Pokud jste povolili hello automatické vytvoření účtu úložiště, pokračujte příliš[krok 2: registrační klíč služby hello Get](#step-2-get-the-service-registration-key).
> 
> 

## <a name="step-2-get-hello-service-registration-key"></a>Krok 2: Získání registračního klíče služby hello

Po hello služby StorSimple Manager zařízení je spuštěný a funkční, budete potřebovat registrační klíč služby hello tooget. Tento klíč je použité tooregister a připojení zařízení StorSimple službou hello.

Proveďte následující kroky v hello hello [portál Azure](https://portal.azure.com/).

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> Hello registrační klíč služby je použité tooregister všechny hello zařízení StorSimple Manager zařízení, která vyžadují tooregister s služby StorSimple Manager zařízení.
> 
> 

## <a name="step-3-download-hello-virtual-array-image"></a>Krok 3: Stáhnout bitovou kopii virtuálního pole hello

Až budete mít registrační klíč služby hello, budete potřebovat toodownload hello příslušné virtuální pole image tooprovision virtuální pole v systému hostitele. Hello virtuální pole Image jsou specifické pro operační systém a lze ji stáhnout ze stránky rychlý Start hello v hello portálu Azure.

> [!IMPORTANT]
> Hello software spuštěný na hello pole virtuální zařízení StorSimple lze použít pouze s hello služby StorSimple Manager zařízení.
> 
> 

Proveďte následující kroky v hello hello [portál Azure](https://portal.azure.com/).

#### <a name="tooget-hello-virtual-array-image"></a>Obrázek virtuální pole tooget hello

1. Přihlaste se k hello [portál Azure](https://portal.azure.com/). 
2. V hello portálu Azure, klikněte na **procházet > Správci zařízení StorSimple**.
3. Vyberte existující službu StorSimple Manager zařízení. V hello **Manager zařízení StorSimple** okně klikněte na tlačítko **rychlý Start**. 
4. Klikněte na tlačítko hello odkaz odpovídající toohello bitové kopie, které chcete toodownload z hello Microsoft Download Center. soubory Hello image jsou přibližně 4,8 GB.
   
   * VHDX pro Hyper-V v systému Windows Server 2012 a novější
   * Virtuální pevný disk pro technologii Hyper-V v systému Windows Server 2008 R2 a novější
   * VMDK pro VMWare ESXi 5.5 a novější
5. Stáhněte a rozbalte hello souboru tooa místní disk, provádění poznamenejte si kde je umístěn soubor rozbalené hello.

## <a name="optional-step-configure-a-new-storage-account-for-hello-service"></a>Volitelný krok: Konfigurace nového účtu úložiště pro službu hello

Tento krok je volitelný a musí provést pouze v případě, že jste nepovolili automatické vytvoření účtu úložiště hello s vaší služby.

Pokud potřebujete toocreate účet úložiště Azure v jiné oblasti, přečtěte si [jak toocreate účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) podrobné pokyny.

Proveďte následující kroky v hello hello [portál Azure](https://ms.portal.azure.com/) na hello Správce zařízení StorSimple služby stránky tooadd stávající účet úložiště Microsoft Azure.

#### <a name="tooadd-a-storage-account-credential-that-has-hello-same-azure-subscription-as-hello-device-manager-service"></a>hello tooadd pověření účtu úložiště, který má stejné předplatné jako služba hello Správce zařízení

1. Přejděte tooyour služby Správce zařízení, vyberte a dvakrát na ni klikněte. Tím se otevře hello **přehled** okno.
2. Vyberte **přihlašovacích údajů účtu úložiště** v rámci hello **konfigurace** části.
3. Klikněte na tlačítko **Přidat**.
4. V hello **přidání účtu úložiště** okně hello následující:
   
    1. Pro **předplatné**, vyberte **aktuální**.
   
    2. Zadejte název hello svého účtu úložiště Azure.
   
    3. Vyberte **povolit** toocreate zabezpečený kanál pro síťovou komunikaci mezi StorSimple zařízení a hello cloudu. Vyberte **zakázat** pouze v případě, že pracujete v privátním cloudu.
   
    4. Klikněte na tlačítko **Přidat**. Upozornění se zobrazí po úspěšném vytvoření účtu úložiště hello.<br></br>
   
     ![Přidat existující přihlašovací údaje účtu úložiště](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a>Další krok

dalším krokem Hello je tooprovision virtuálního počítače pro pole virtuální zařízení StorSimple. V závislosti na operačním systému hostitele najdete podrobné pokyny v hello:

* [Zřídit o virtuální zařízení StorSimple pole technologie Hyper-v](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [Zřídit o virtuální zařízení StorSimple pole v prostředí VMware](storsimple-virtual-array-deploy2-provision-vmware.md)

