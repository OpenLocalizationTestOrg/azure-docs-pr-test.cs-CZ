---
title: "Podpora klienta aaaMulti tooAzure replikace virtuálního počítače VMware (CSP program) | Microsoft Docs"
description: "Popisuje, jak toodeploy Azure Site Recovery v prostředí více klientů tooorchestrate replikace, převzetí služeb při selhání a obnovení místní virtuální počítače (VM) tooAzure VMware prostřednictvím programu hello CSP pomocí hello portálu Azure"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: manayar
ms.openlocfilehash: 9be555c9a438f66e6d3dfcdc9f507a84763846d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="multi-tenant-support-in-azure-site-recovery-for-replicating-vmware-virtual-machines-tooazure-through-csp"></a>Podpora více klientů ve službě Azure Site Recovery pro replikaci tooAzure virtuální počítače VMware prostřednictvím zprostředkovatele kryptografických služeb

Azure Site Recovery podporuje víceklientské prostředí odběrů klienta. Podporuje také více klientů pro klienta předplatné, které se vytváří a spravují prostřednictvím programu Microsoft Cloud Solution Provider (CSP) hello. Tento článek podrobně hello pokyny pro implementaci a správu scénáře víceklientské VMware do Azure. Také vysvětluje vytváření a Správa předplatného klienta prostřednictvím poskytovatele CSP.

V tomto návodu nevykresluje výraznou z hello stávající dokumentaci pro replikaci tooAzure virtuálních počítačů VMware. Další informace najdete v tématu [tooAzure virtuální počítače VMware replikovat pomocí Site Recovery](site-recovery-vmware-to-azure.md).

## <a name="multi-tenant-environments"></a>Víceklientské prostředí
Existují tři hlavní modely více klientů:

* **Sdílené hostování poskytovatele služeb (HSP)**: hello partnera vlastní hello fyzické infrastruktury a používá sdílené prostředky (vCenter, datových center, fyzického úložiště a tak dále) toohost hello virtuálních počítačů více klienty ve stejné infrastruktuře. Hello partner můžete poskytovat zotavení po havárii správu jako spravované služby nebo hello klienta můžete vlastní zotavení po havárii jako řešení samoobslužné služby.

* **Vyhrazené hostování zprostředkovatele služeb**: hello partnera vlastní vyhrazených prostředcích (více Vcenter, fyzické datastores a tak dále), ale používá fyzické infrastruktuře hello toohost každého klienta virtuálních počítačů na infrastruktuře samostatné. Hello partner můžete poskytovat zotavení po havárii správu jako spravované služby nebo hello klienta můžete vlastní jako řešení samoobslužné služby.

* **Spravované služby zprostředkovatele (MSP)**: hello zákazník vlastní hello fyzické infrastruktury, který je hostitelem hello virtuální počítače a hello partnera poskytuje povolování zotavení po havárii a pro správu.

## <a name="shared-hosting-multi-tenant-guidance"></a>Sdílené hostování víceklientské pokyny
Tato část popisuje scénář sdílené hostování hello podrobně. Hello další dva scénáře jsou podmnožinou tohoto scénáře hostování sdílených hello a používají hello stejnými zásadami. na konci hello hello sdílené hostování pokyny jsou popsané rozdíly Hello.

Hello základní požadavek ve scénáři víceklientské je tooisolate hello různé klienty. Jeden klienta by nemělo být schopný tooobserve, co má atribut hosted jiného klienta. Tento požadavek není důležité, protože je v samoobslužné prostředí, kde může být rozhodující, v prostředí spravovaná partnerem. Tyto pokyny předpokládají, že je vyžadována izolaci klientů.

Architektura Hello jsou poskytovány hello následující diagram:

![Sdílené HSP s jeden vCenter](./media/site-recovery-multi-tenant-support-vmware-using-csp/shared-hosting-scenario.png)  
**Sdílené hostování scénář se jeden vCenter**

Jak je vidět v předchozím diagramu hello, každý zákazník má server pro správu samostatné. Konfigurace omezení klientovi přístup k virtuálním počítačům tootenant specifické a umožňuje izolaci klientů. Scénář replikaci virtuálního počítače VMware používá hello konfigurace serveru toomanage účty toodiscover virtuální počítače a nainstalovat agenty. Budeme postupovat podle hello řízení přístupu se stejnými zásadami pro prostředí s více klientů, uveďte hello omezení zjišťování virtuálních počítačů pomocí systému vCenter.

požadavek na data izolace Hello vyžaduje, aby podmínky tootenants zůstanou všechny informace o citlivých infrastruktury (například přihlašovací údaje). Z tohoto důvodu doporučujeme, aby všechny součásti serveru pro správu hello zůstaly ve hello výhradní kontrolu nad hello partnera. součásti serveru pro správu Hello jsou:
* Konfigurační server (CS)
* Procesový server (PS)
* Hlavní cílový server (MT) 

PS Škálováním na více systémů je také pod kontrolou hello partnera.

### <a name="every-cs-in-hello-multi-tenant-scenario-uses-two-accounts"></a>Každý CS ve scénáři víceklientské hello používá dva účty

- **účet pro přístup k systému vCenter**: pomocí tohoto účtu toodiscover klienta virtuálních počítačů. Má oprávnění k přístupu vCenter přiřazené tooit (jak je popsáno v další části hello). toohelp vyhnout přístup náhodných úniků, doporučujeme, aby partneři zadejte tyto přihlašovací údaje, sami v konfiguračním nástroji služby hello.

- **Účet pro přístup k virtuálnímu počítači**: pomocí tohoto účtu tooinstall hello mobility agenta na hello klientské virtuální počítače pomocí automatické nabízené. Je obvykle účet domény, že klient může poskytovat tooa partnera nebo že, případně hello partnera může spravovat přímo. Pokud klient nechce tooshare hello podrobnosti s partnerem hello přímo, potvrdí můžete povolen, přihlašovací údaje hello tooenter prostřednictvím časově omezenou přístup k toohello CS nebo partnera hello pomoc při instalaci agentů mobility ručně.

### <a name="requirements-for-a-vcenter-access-account"></a>Požadavky pro účet přístupu k systému vCenter

Jak je uvedeno v předcházející části hello, musíte nakonfigurovat účet, který má přiřazenou roli speciální tooit hello CS. přiřazení role Hello musí být použít účet přístupu k toohello vCenter pro každý objekt vCenter a není rozšíří toohello podřízené objekty. Tato konfigurace zajišťuje izolaci klientů, vzhledem k šíření přístup může být v objektech tooother náhodnému přístup.

![Hello Propagate tooChild objekty možnost](./media/site-recovery-multi-tenant-support-vmware-using-csp/assign-permissions-without-propagation.png)

alternativní Hello je tooassign hello uživatelský účet a role v objektu hello datacenter a šířit je toohello podřízené objekty. Pak umožnit hello účet *žádný přístup* role pro každý objekt (například virtuální počítače ostatních klientů), který by měl být nedostupné tooa konkrétní klienta. Tato konfigurace je náročná a zpřístupňuje ovládacích prvků náhodných přístup, protože každý nový podřízený objekt je také automaticky udělen přístup, který dědí z nadřazeného hello. Proto doporučujeme použít první metodu hello.

Hello vCenter přístup účtu postup je následující:

1. Vytvoření nové role klonováním hello předem definované *jen pro čtení* role a pak mu pohodlný název (například Azure_Site_Recovery, jak je znázorněno v tomto příkladu).

2. Přiřaďte následující oprávnění role toothis hello:

    * **Úložiště dat**: přidělit prostor, procházet úložiště, operace nízké úrovně souborů, odebrat soubor, soubory aktualizace virtuálního počítače

    * **Sítě**: sítě přiřazení

    * **Prostředek**: fondu tooresource přiřazení virtuálního počítače, migraci vypnout virtuální počítač, migrace zapnutý virtuálních počítačů

    * **Úlohy**: vytvoření úlohy, úloha aktualizace

    * **Virtuální počítač**: 
        * Konfigurace > všechny
        * Interakce > odpovědět na otázku, připojení zařízení, nakonfigurovat CD média, konfigurovat disketová média, vypnutí napájení, instalaci nástroje VMware
        * Inventář > vytvořit z existujícího, vytvořit nový, registraci, zrušení registrace
        * Zřizování > Povolit stahování virtuálního počítače, nahrávání souborů virtuálního počítače povolit
        * Správa snímků > odebrat snímky

    ![Dialogové okno Upravit roli Hello](./media/site-recovery-multi-tenant-support-vmware-using-csp/edit-role-permissions.png)

3. Přiřazení úrovní toohello vCenter účet pro přístup k (používá se v klientovi hello CS) pro různé objekty, následujícím způsobem:

>| Objekt | Role | Poznámky |
>| --- | --- | --- |
>| vCenter | Jen pro čtení | Potřeba pouze vCenter tooallow přístup pro správu různé objekty. Toto oprávnění může odebrat, pokud účet hello přechází nikdy toobe poskytuje tooa klienta nebo použít pro žádné operace správy na hello vCenter. |
>| Datacentrum | Azure_Site_Recovery |  |
>| Hostitele a cluster hostitelů | Azure_Site_Recovery | Znovu zajišťuje, že přístup na úrovni objektu hello tak, aby dostupné jenom hostitelé klientské virtuální počítače před převzetí služeb při selhání a po navrácení služeb po obnovení. |
>| Úložiště dat a úložiště clusteru | Azure_Site_Recovery | Stejné jako předcházející. |
>| Síť | Azure_Site_Recovery |  |
>| Server pro správu | Azure_Site_Recovery | Zahrnuje přístup tooall součásti (CS, PS a MT), pokud jsou některé mimo počítač hello CS. |
>| Virtuální počítače klienta | Azure_Site_Recovery | Zajišťuje, že všechny nové klientské virtuální počítače konkrétního klienta také získat tento přístup, nebo nebude zjistitelný prostřednictvím hello portálu Azure. |

přístup k účtu vCenter Hello je nyní dokončen. Tento krok plnit hello minimální oprávnění požadavek toocomplete navrácení služeb po obnovení operace. Můžete taky těchto oprávnění k přístupu s existující zásady. Právě oprávnění k úpravě vaší existující oprávnění sady tooinclude role z kroku 2, podrobné dříve.

operace zotavení po havárii toorestrict dokud hello stav převzetí služeb při selhání (tedy bez možnosti navrácení služeb po obnovení), postupujte podle hello předchozím postupu, s výjimkou: místo přiřazení hello *Azure_Site_Recovery* role účet pro přístup k systému vCenter toohello, přiřadit jenom *jen pro čtení* toothat účtu s rolí. Této sadě oprávnění umožňuje replikace virtuálního počítače a převzetí služeb při selhání a navrácení služeb po obnovení není povolen. Všem ostatním v hello předcházející procesu zůstane, jako je. tooensure izolaci klientů a omezte zjišťování virtuálních počítačů, každý oprávnění stále přidělený na hello objekt úrovni pouze a není šířený toochild objekty.

## <a name="other-multi-tenant-environments"></a>Dalších prostředích s více klienty

Hello předcházející části popisuje jak tooset víceklientské prostředí pro sdílené hostování řešení. Hello další dvě hlavní řešení jsou vyhrazené hostování a spravované služby. Architektura Hello těchto řešení je popsána v následující části hello.

### <a name="dedicated-hosting-solution"></a>Vyhrazené řešení v oblasti hostování

Jak ukazuje následující diagram hello, hello architektury rozdíl ve vyhrazené hostingu řešení je, je pro tohoto klienta pouze nastavili infrastruktury každého klienta. Vzhledem k tomu, že klienti jsou izolované prostřednictvím samostatné Vcenter, poskytovatele hostingu hello musí stále postupujte podle kroků CSP hello zadaná pro sdílené hostování ale nemusí tooworry o izolaci klientů. Instalační program zprostředkovatele kryptografických služeb zůstává beze změny.

![Architektura sdílené hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/dedicated-hosting-scenario.png)  
**Vyhrazený hostitelský scénář s více Vcenter**

### <a name="managed-service-solution"></a>Řešení spravované služby

Jako hello uvedené v následujícím diagramu, hello architektury rozdíl v řešení spravované služby je každý klient infrastruktury je také fyzicky oddělená od ostatních klientů infrastruktury. Tento scénář existuje obvykle při hello klienta vlastní hello infrastruktury a chce zotavení po havárii toomanage poskytovatele řešení. Znovu protože klienti jsou fyzicky izolované prostřednictvím různých infrastruktury, hello partnera vyžaduje toofollow hello CSP kroků uvedených pro sdílené hostování, ale nemusí tooworry o izolaci klientů. Zřizování CSP zůstává beze změny.

![Architektura sdílené hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/managed-service-scenario.png)  
**Spravované služby scénář s více Vcenter**

## <a name="csp-program-overview"></a>Přehled programu zprostředkovatele kryptografických služeb
Hello [CSP program](https://partner.microsoft.com/en-US/cloud-solution-provider) podporovalo lepší společně scénářů, které nabízejí partnery všem cloudovým službám Microsoftu, včetně služeb Office 365, Enterprise Mobility Suite a Microsoft Azure. S CSP našimi partnery vlastní hello začátku do konce vztahů se zákazníky a přestat hello vztah primární kontaktní bod. Partnery můžete nasadit předplatná Azure pro zákazníky a kombinovat hello předplatných s vlastní s přidanou hodnotou, přizpůsobené nabídky.

Azure Site Recovery partneři můžou spravovat hello kompletního řešení zotavení po havárii pro zákazníky přímo přes zprostředkovatele kryptografických služeb. Nebo můžete použít CSP tooset prostředí Site Recovery a Informujte zákazníky, spravovat jejich vlastní potřeby zotavení po havárii způsobem samoobslužné služby. V obou případech partneři jsou hello spolupráci mezi Site Recovery a zákazníků. Partneři služby hello vztah se zákazníkem a vyúčtování pro zákazníky za použití Site Recovery.

## <a name="create-and-manage-tenant-accounts"></a>Vytvořit a spravovat účty klientů

### <a name="step-0-prerequisite-check"></a>Krok 0: Kontrola předpokladů

Hello požadavky virtuálního počítače jsou hello stejný jako v hello [dokumentace Azure Site Recovery](site-recovery-vmware-to-azure.md). Přidání toothose požadavky by měl můžete mít hello výše uvedených přístup ovládací prvky zavedené předtím, než budete pokračovat se správou klientů prostřednictvím poskytovatele CSP. Pro každého klienta vytvořte samostatné management server, který může komunikovat s hello klientské virtuální počítače a jeho vCenter. Pouze hello partner má server toothis oprávnění přístupu.

### <a name="step-1-create-a-tenant-account"></a>Krok 1: Vytvoření účtu klienta

1. Prostřednictvím [Microsoft Partner Center](https://partnercenter.microsoft.com/), přihlašovací účet tooyour CSP. 
 
2. Na hello **řídicí panel** nabídce vyberte možnost **zákazníci**.

    ![odkaz Hello zákazníků Center partnera společnosti Microsoft](./media/site-recovery-multi-tenant-support-vmware-using-csp/csp-dashboard-display.png)

3. Na stránce hello, které se otevře, klikněte na hello **přidat zákazníka** tlačítko.

    ![tlačítko Přidat zákazník Hello](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-new-customer.png)

4. Na hello **nového zákazníka** stránky, vyplňte hello podrobnosti informace o účtu pro hello klienta a pak klikněte na tlačítko **Další: odběry**.

    ![stránka informace o účtu Hello](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-add-filled.png)

5. Na stránce Výběr hello předplatných, vyberte hello **Microsoft Azure** zaškrtávací políčko. Můžete přidat další odběry nyní nebo kdykoli později.

    ![políčko předplatné Hello Microsoft Azure](./media/site-recovery-multi-tenant-support-vmware-using-csp/azure-subscription-selection.png)

6. Na hello **zkontrolujte** Potvrďte podrobnosti hello klienta a pak klikněte na tlačítko **odeslání**.

    ![Kontrolní stránku Hello](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-summary-page.png)  

    Po vytvoření účtu klienta hello, zobrazí se stránka potvrzení, zobrazení podrobností o hello Dobrý den výchozí účet a heslo pro toto předplatné hello. 

7. Uložte informace hello a změnit heslo hello později podle potřeby prostřednictvím hello Azure přihlašovací stránky portálu.  
 
    Tyto informace můžete sdílet s hello klienta, jako je, nebo můžete vytvořit a sdílet samostatný účet, v případě potřeby.

### <a name="step-2-access-hello-tenant-account"></a>Krok 2: Přístup k účtu klienta hello

Předplatného hello klienta můžete přistupovat prostřednictvím hello panelu Center Microsoft Partner, jak je popsáno v "krok 1: vytvoření účtu klienta." 

1. Přejděte toohello **zákazníci** a pak klikněte na tlačítko hello název účtu klienta hello.

2. Na hello **odběry** stránku hello účtu klienta, můžete monitorovat hello existující účet odběry a podle potřeby přidejte více odběrů. operace zotavení po havárii toomanage hello klienta, vyberte **všechny prostředky (portál Azure)**.

    ![Všechny prostředky Hello propojení](./media/site-recovery-multi-tenant-support-vmware-using-csp/all-resources-select.png)  
    
    Kliknutím na tlačítko **všechny prostředky** uděluje přístup předplatná Azure toohello klienta. Přístup můžete ověřit kliknutím na odkaz hello Azure Active Directory v hello top napravo od hello portálu Azure.

    ![Azure Active Directory odkaz](./media/site-recovery-multi-tenant-support-vmware-using-csp/aad-admin-display.png)

Teď můžete provádět všechny operace obnovení lokality pro klienta hello prostřednictvím hello portál Azure a spravovat operace zotavení po havárii hello. tooaccess hello klienta předplatné přes zprostředkovatele kryptografických služeb pro zotavení po havárii spravované, postupujte podle hello dříve popsané procesu.

### <a name="step-3-deploy-resources-toohello-tenant-subscription"></a>Krok 3: Nasaďte prostředky toohello tenanta předplatného
1. Na hello portálu Azure vytvořte skupinu prostředků a pak nasadit trezoru služeb zotavení pro obvyklé procesy hello. 
 
2. Stáhněte si registrační klíč trezoru hello.

3. Zaregistrujte hello CS pro hello klienta pomocí registračního klíče trezoru hello.

4. Zadejte přihlašovací údaje hello hello dva přístupové účty: účet pro přístup k systému vCenter a virtuální počítač přístup k účtu.

    ![Účty serveru configuration Manager](./media/site-recovery-multi-tenant-support-vmware-using-csp/config-server-account-display.png)

### <a name="step-4-register-site-recovery-infrastructure-toohello-recovery-services-vault"></a>Krok 4: Registrace Site Recovery infrastruktury služby zotavení toohello trezoru
1. V hello portál Azure, na hello trezoru, který jste vytvořili dříve, zaregistrujte hello vCenter server toohello CS, který je zaregistrovaný v "krok 3: nasazení prostředků toohello tenanta předplatného." Použijte účet přístupu k hello vCenter pro tento účel.
2. Dokončení procesu "Připravit infrastrukturu" hello Site Recovery pro obvyklé procesy hello.
3. virtuální počítače Hello jsou nyní připraven toobe replikovat. Ověřte, že pouze hello virtuálních počítačů klienta se zobrazují na hello **vybrat virtuální počítače** okno pod hello **replikovat** možnost.

    ![Seznam virtuálních počítačů klienta v okně vyberte virtuální počítače hello](./media/site-recovery-multi-tenant-support-vmware-using-csp/tenant-vm-display.png)

### <a name="step-5-assign-tenant-access-toohello-subscription"></a>Krok 5: Přístup toohello předplatné přiřazení klienta

Samoobslužné služby zotavení po havárii, zadejte toohello klienta hello účet podrobnosti, jak je uvedeno v kroku 6 hello "krok 1: vytvoření účtu klienta" oddílu. Tuto akci proveďte, jakmile hello partnera nastaví infrastruktury hello zotavení po havárii. Jestli je typ zotavení po havárii hello spravované nebo samoobslužné služby, partneři musí získat přístup k klienta odběry prostřednictvím portálu CSP hello. Jejich nastavení pro zařízení vlastněná partnera trezoru hello a zaregistrujte infrastruktury toohello klienta odběry.

Partnery můžete také přidat nové předplatné klienta toohello uživatele prostřednictvím portálu hello zprostředkovatele kryptografických služeb díky hello následující:

1. Přejděte na stránku odběru CSP toohello klienta a potom vyberte hello **uživatelé a licence** možnost.

    ![Hello stránku odběru CSP klienta](./media/site-recovery-multi-tenant-support-vmware-using-csp/users-and-licences.png)

    Nyní můžete vytvořit nového uživatele zadáním příslušné podrobnosti hello a výběrem oprávnění nebo tím, že nahrajete hello seznam uživatelů v souboru CSV.

2. Po vytvoření nového uživatele, přejděte zpět toohello Azure portálu a potom na hello **předplatné** okně, vyberte hello příslušné předplatné.

3. V okně hello, které se otevře, vyberte **řízení přístupu (IAM)**a potom klikněte na **přidat** tooadd uživatele, který má úroveň přístupu relevantní hello.      
    Hello uživatelů, které byly vytvořeny prostřednictvím portálu CSP hello se automaticky zobrazí v okně hello, které se otevře po kliknutí na tlačítko úroveň přístupu.

    ![Přidání uživatele](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-user-subscription.png)

    Pro většinu operací správy hello *Přispěvatel* role je dostačující. Uživatelé s touto úrovní přístupu může dělat všechno na předplatné s výjimkou Změna úrovně přístupu (pro které *vlastníka*– úroveň přístupu se vyžaduje). Můžete také optimalizovat hello úrovně přístupu podle potřeby.
