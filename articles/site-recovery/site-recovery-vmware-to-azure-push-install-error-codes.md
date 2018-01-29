---
title: "Azure Site Recovery řešení potíží s z VMware do Azure | Microsoft Docs"
description: "Řešení chyb při replikaci virtuálních počítačů Azure"
services: site-recovery
author: anoopkv
manager: gauravd
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: anoopkv
ms.openlocfilehash: c5566ec44a8bfed0a3e7207c2cebf21517573541
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Služba Mobility nabízené řešení potíží s instalací

Tento článek popisuje běžné problémy, která vám může při instalaci službě Azure Site Recovery Mobility na zdrojovém serveru pro povolení ochrany.

## <a name="error-78007---the-requested-operation-could-not-be-completed"></a>Chyba 78007 - požadovanou operaci nelze dokončit.
Tato chyba může být vyvolána službou z několika důvodů. Zvolte odpovídající chyby zprostředkovatele další řešení.

* [Error 95103](#error-95103---protection-could-not-be-enabled-ep0854) 
* [Chyba 95105](#error-95105---protection-could-not-be-enabled-ep0856) 
* [Chyba 95107](#error-95107---protection-could-not-be-enabled-ep0858) 
* [Chyba 95108](#error-95108---protection-could-not-be-enabled-ep0859) 
* [Chyba 95117](#error-95117---protection-could-not-be-enabled-ep0865) 
* [Chyba 95213](#error-95213---protection-could-not-be-enabled-ep0874) 
* [Chyba 95224](#error-95224---protection-could-not-be-enabled-ep0883) 
* [Chyba 95265](#error-95265---protection-could-not-be-enabled-ep0902) 


## <a name="error-95105---protection-could-not-be-enabled-ep0856"></a>Chyba 95105 - ochranu nelze povolit (EP0856)

**Kód chyby** | **Možné příčiny** | **Chyba konkrétní doporučení**
--- | --- | ---
95105 </br>**Zpráva:** nabízenou instalaci služby Mobility na zdrojovém počítači se nezdařilo s kódem chyby **EP0856**. <br> Buď **sdílení souborů a tiskáren** není povolená pro zdroj počítače nebo že jsou síťové potíže s připojením k mezi procesovým serverem a zdrojovým počítačem.| **Sdílení souborů a tiskáren** není povoleno. | Povolit **sdílení souborů a tiskáren** na zdrojovém počítači v bráně Windows Firewall. Na zdrojovém počítači v části **brány Windows Firewall** > **povolit aplikace nebo funkci průchod bránou Firewall**, vyberte **sdílení souborů a tiskáren pro všechny profily**. </br> Kromě toho zkontrolujte splnění následujících předpokladů úspěšné dokončení nabízené instalace.<br> Další informace o [řešení potíží s isssues rozhraní WMI](#troubleshoot-wmi-issues)


## <a name="error-95107---protection-could-not-be-enabled-ep0858"></a>Chyba 95107 - ochranu nelze povolit (EP0858)

**Kód chyby** | **Možné příčiny** | **Chyba konkrétní doporučení**
--- | --- | ---
95107 </br>**Zpráva:** nabízenou instalaci služby Mobility na zdrojovém počítači se nezdařilo s kódem chyby **EP0858**. <br> Buď jsou nesprávné přihlašovací údaje zadané pro instalaci služby Mobility nebo uživatelský účet nemá dostatečná oprávnění. | Pověření uživatele zadané pro instalaci služby Mobility na zdrojovém počítači jsou nesprávné. | Zkontrolujte správnost zadané pověření uživatele pro zdrojový počítač na konfiguračním serveru. <br> Můžete přidat nebo upravit přihlašovací údaje uživatele, přejděte na konfigurační server a vyberte **Cspsconfigtool** > **spravovat účet**. </br> Kromě toho zkontrolujte následující [požadavky](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) úspěšné dokončení nabízené instalace.


## <a name="error-95117---protection-could-not-be-enabled-ep0865"></a>Chyba 95117 - ochranu nelze povolit (EP0865)

**Kód chyby** | **Možné příčiny** | **Chyba konkrétní doporučení**
--- | --- | ---
95117 </br>**Zpráva:** nabízenou instalaci služby Mobility na zdrojovém počítači se nezdařilo s kódem chyby **EP0865**. <br> Buď zdrojový počítač není spuštěný nebo jsou potíže s připojením k síti mezi procesovým serverem a zdrojovým počítačem. | Potíže s připojením k síti mezi procesovým serverem a zdrojového serveru. | Zkontrolujte připojení mezi procesovým serverem a zdrojového serveru. </br> Kromě toho zkontrolujte následující [požadavky](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) úspěšné dokončení nabízené instalace.|

## <a name="error-95103---protection-could-not-be-enabled-ep0854"></a>Chyba 95103 - ochranu nelze povolit (EP0854)

**Kód chyby** | **Možné příčiny** | **Chyba konkrétní doporučení**
--- | --- | ---
95103 </br>**Zpráva:** nabízenou instalaci služby Mobility na zdrojovém počítači se nezdařilo s kódem chyby **EP0854**. <br> Buď není povolen Windows Management Instrumentation (WMI) na zdrojovém počítači, nebo nejsou potíže s připojením k síti mezi procesovým serverem a zdrojovým počítačem.| V bráně Windows Firewall je blokována rozhraní WMI. | Povolit rozhraní WMI v bráně Windows Firewall. V části **brány Windows Firewall** > **povolit aplikace nebo funkci průchod bránou Firewall**, vyberte **rozhraní WMI pro všechny profily**. </br> Kromě toho zkontrolujte následující [požadavky](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) úspěšné dokončení nabízené instalace.|

## <a name="error-95213---protection-could-not-be-enabled-ep0874"></a>Chyba 95213 - ochranu nelze povolit (EP0874)

**Kód chyby** | **Možné příčiny** | **Chyba konkrétní doporučení**
--- | --- | ---
95213 </br>**Zpráva:** instalace služby mobility selhala na zdrojovém počítači % SourceIP; selhala s kódem chyby **EP0874**. <br> | Verze operačního systému na zdrojovém počítači není podporována. <br>| Zajistěte, aby zdrojového počítače, které podporuje verze operačního systému. Pro čtení [matici podpory](https://aka.ms/asr-os-support). </br> Kromě toho zkontrolujte následující [požadavky](https://aka.ms/pushinstallerror) úspěšné dokončení nabízené instalace.| 


## <a name="error-95108---protection-could-not-be-enabled-ep0859"></a>Chyba 95108 - ochranu nelze povolit (EP0859)

**Kód chyby** | **Možné příčiny** | **Chyba konkrétní doporučení**
--- | --- | ---
95108 </br>**Zpráva:** nabízenou instalaci služby mobility na zdrojovém počítači se nezdařilo s kódem chyby **EP0859**. <br>| Že přihlašovací údaje poskytnuté pro instalaci služby mobility je nesprávný nebo uživatelský účet nemá dostatečná oprávnění <br>| Zajistěte, aby zadaná pověření **kořenové** přihlašovací údaje účtu. Pokud chcete přidat či upravit přihlašovací údaje uživatele, přejděte na konfigurační server a klikněte na ikonu "Cspsconfigtool" zástupce na ploše. Klikněte na "Účet spravovat" můžete přidat nebo upravit přihlašovací údaje.|

## <a name="error-95265---protection-could-not-be-enabled-ep0902"></a>Chyba 95265 - ochranu nelze povolit (EP0902)

**Kód chyby** | **Možné příčiny** | **Chyba konkrétní doporučení**
--- | --- | ---
95265 </br>**Zpráva:** nabízenou instalaci služby mobility na zdrojový počítač se úspěšně, ale zdrojový počítač vyžaduje restartování pro některé systému změny vstoupily v platnost. <br>| Na serveru je již nainstalována starší verze služby mobility.| Plynule pokračuje replikace virtuálního počítače.<br> Restartujte server vaší další období údržby, chcete-li získat výhody nová vylepšení v služba mobility.|


## <a name="error-95224---protection-could-not-be-enabled-ep0883"></a>Chyba 95224 - ochranu nelze povolit (EP0883)

**Kód chyby** | **Možné příčiny** | **Chyba konkrétní doporučení**
--- | --- | ---
95224 </br>**Zpráva:** nabízená instalace služby mobility na zdrojový počítač % SourceIP; selhala s kódem chyby EP0883. Systém restartovat z předchozí instalace / aktualizace čeká na vyřízení.| Odinstalovává se starší nebo nekompatibilní verze služby mobility nebyl restartovat systém.| Ujistěte se, že neexistuje žádná verze služby mobility na serveru. <br> Restartujte server a znovu spusťte úlohu povolení ochrany|

## <a name="resource-to-troubleshoot-push-installation-problems"></a>Prostředek k odstraňování problémů instalace push

#### <a name="troubleshoot-file-and-print-sharing-issues"></a>Řešení problémů sdílení souborů a tiskáren
*  [Povolit nebo zakázat sdílení souborů pomocí zásad skupiny](https://technet.microsoft.com/library/cc754359(v=ws.10).aspx)
* [Povolení souboru a tisku sdílení přes bránu Windows Firewall](https://technet.microsoft.com/library/ff633412(v=ws.10).aspx)

#### <a name="troubleshoot-wmi-issues"></a>Řešení problémů WMI
* [Testování základní rozhraní WMI](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [Řešení problémů WMI](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [Řešení potíží s skripty WMI a služby WMI](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="next-steps"></a>Další postup

[Zjistěte, jak](tutorial-vmware-to-azure.md) nastavit zotavení po havárii pro virtuální počítače VMware.