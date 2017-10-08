---
title: aaaHPC Pack cluster s Azure Active Directory | Microsoft Docs
description: "Zjistěte, jak toointegrate HPC Pack 2016 cluster v Azure s Azure Active Directory"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
ms.assetid: 9edf9559-db02-438b-8268-a6cba7b5c8b7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 11/14/2016
ms.author: danlep
ms.openlocfilehash: 0806e544a468e27ca0567e18c55554811584fbc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-hpc-pack-cluster-in-azure-using-azure-active-directory"></a>Spravovat cluster služby HPC Pack v Azure pomocí Azure Active Directory
[Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) podporuje integraci se službou [Azure Active Directory](../../active-directory/index.md) (Azure AD) pro správce, kteří nasazení clusteru HPC Pack v Azure.



Postupujte podle kroků hello v tomto článku hello následující úlohy vysoké úrovně: 
* Cluster HPC Pack ručně integrate klientovi Azure AD
* Spravovat a plánování úloh v clusteru HPC Pack v Azure 

Integrace s Azure AD řešení clusteru prostředí HPC Pack následuje toointegrate standardní kroky, ostatní aplikace a služby. Tento článek předpokládá, že jste se seznámili s základní uživatel správy ve službě Azure AD. Další informace a pozadí, najdete v části hello [dokumentaci služby Azure Active Directory](../../active-directory/index.md) a hello následující části.

## <a name="benefits-of-integration"></a>Výhody integrace


Azure Active Directory (Azure AD) je víceklientské cloudové adresáře a identity management služba, která poskytuje jednotné přihlašování (SSO) přístup toocloud řešení.

Integrace clusteru HPC Pack s Azure AD můžete dosáhnout hello následující cíle:

* Odeberte z clusteru HPC Pack hello hello tradiční řadič domény služby Active Directory. To může pomoct snížit náklady na hello Správa clusteru hello, pokud to není nezbytné pro své firmy a proces nasazení hello zrychlit.
* Využít hello následující výhody, které jsou uvedeny ve službě Azure AD:
    *   Jednotné přihlašování 
    *   Pomocí místní AD identity pro cluster HPC Pack hello v Azure 

    ![Prostředí Azure Active Directory](./media/hpcpack-cluster-active-directory/aad.png)


## <a name="prerequisites"></a>Požadavky
* **Cluster HPC Pack 2016 nasazené ve virtuálních počítačích Azure** – pokyny najdete v tématu [nasazení clusteru HPC Pack 2016 v Azure](hpcpack-2016-cluster.md). Potřebujete název DNS hello hello hlavního uzlu a hello přihlašovací údaje Správce clusteru dokončit hello kroky v tomto článku.

  > [!NOTE]
  > Integrace se službou Azure Active Directory není podporováno ve verzích sady HPC Pack před HPC Pack 2016.



* **Klientský počítač** -je nutné Windows nebo Windows Server klientské počítače příliš spustit HPC Pack klienta nástroje. Pokud chcete pouze toouse hello HPC Pack webový portál nebo REST API toosubmit úloh, můžete použít libovolného klientského počítače podle svého výběru.

* **HPC Pack klienta nástroje** -nainstalujte na hello klientského počítače, pomocí hello volné instalačního balíčku k dispozici z hello Microsoft Download Center hello HPC Pack klienta nástroje.


## <a name="step-1-register-hello-hpc-cluster-server-with-your-azure-ad-tenant"></a>Krok 1: Zaregistrujte server clusteru HPC hello se klientovi služby Azure AD
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Klikněte na tlačítko **služby Active Directory** v levé nabídce text hello a potom klikněte na hello požadovaného adresáře v rámci vašeho předplatného. V adresáři hello musí mít oprávnění tooaccess prostředky.
3. Klikněte na tlačítko **uživatelé**a ověřte, zda uživatelských účtů již vytvořené nebo nakonfigurovaná.
4. Klikněte na tlačítko **aplikace** > **přidat**a potom klikněte na **přidat aplikaci, kterou vyvíjí Moje organizace**. Zadejte následující informace v Průvodci hello hello:
    * **Název** -HPCPackClusterServer
    * **Typ** – vyberte **webové aplikace nebo webové rozhraní API**
    * **Adresa URL přihlašování**– hello základní adresu URL pro hello vzorku, který je ve výchozím nastavení`https://hpcserver`
    * **Identifikátor ID URI aplikace** - `https://<Directory_name>/<application_name>`. Nahraďte `<Directory_name`> s hello celý název klienta služby Azure AD například `hpclocal.onmicrosoft.com`a nahraďte `<application_name>` s názvem hello dříve vybrané.

5. Po přidání aplikace hello klikněte na tlačítko **konfigurace**. Nakonfigurujte hello následující vlastnosti:
    * Vyberte **Ano** pro **aplikace je více klientů**
    * Vyberte **Ano** pro **uživatele přiřazení požadované tooaccess aplikace**.

6. Klikněte na **Uložit**. Po dokončení ukládání, klikněte na tlačítko **spravovat Manifest**. Tato akce stáhne vaší aplikace manifestu JavaScript object notation (JSON) souboru. Upravit hello stáhnout manifest vyhledáním hello `appRoles` nastavení a přidání hello následující aplikační role:
    ```json
    "appRoles": [
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "displayName": "HpcAdminMirror",
        "id": "61e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "description": "HpcAdminMirror",
        "value": "HpcAdminMirror"
        },
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "description": "HpcUsers",
        "displayName": "HpcUsers",
        "id": "91e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "value": "HpcUsers"
        }
    ],
    ```
7. Uložte soubor hello. Hello portálu, klikněte na tlačítko **spravovat Manifest** > **nahrát Manifest**. Potom můžete nahrát hello upravená manifestu.
8. Klikněte na tlačítko **uživatelé**, vyberte uživatele a pak klikněte na tlačítko **přiřadit**. Přiřadíte jeden hello dostupných rolí (HpcUsers nebo HpcAdminMirror) toohello uživatele. Opakujte tento krok s dalším uživatelům v adresáři hello. Základní informace o uživatelích, clusteru, najdete v části [Správa uživatelů clusteru](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).

   > [!NOTE] 
   > toomanage uživatele, doporučujeme použít okno preview služby Azure Active Directory hello v hello [portál Azure](https://portal.azure.com).
   >


## <a name="step-2-register-hello-hpc-cluster-client-with-your-azure-ad-tenant"></a>Krok 2: Registrace klienta clusteru HPC hello se klientovi služby Azure AD

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Klikněte na tlačítko **služby Active Directory** v levé nabídce text hello a potom klikněte na hello požadovaného adresáře v rámci vašeho předplatného. V adresáři hello musí mít oprávnění tooaccess prostředky.
3. Klikněte na tlačítko **aplikace** > **přidat**a potom klikněte na **přidat aplikaci, kterou vyvíjí Moje organizace**. Zadejte následující informace v Průvodci hello hello:

    * **Název** -HPCPackClusterClient
    * **Typ** – vyberte **nativní klientskou aplikaci**
    * **Identifikátor URI pro přesměrování** - `http://hpcclient`

4. Po přidání aplikace hello klikněte na tlačítko **konfigurace**. Kopírování hello **ID klienta** hodnotu a uložte ho. Budete potřebovat později při konfiguraci aplikace.

5. V **oprávnění tooother aplikace**, klikněte na tlačítko **přidat aplikaci**. Vyhledat a přidat aplikaci HpcPackClusterServer hello (vytvořený v kroku 1).

6. V hello **delegovaná oprávnění** rozevíracího seznamu vyberte **přístup HpcClusterServer**. Potom klikněte na **Uložit**.


## <a name="step-3-configure-hello-hpc-cluster"></a>Krok 3: Konfigurace clusteru HPC hello

1. Připojte toohello HPC Pack 2016 hlavního uzlu v Azure.

2. Spusťte prostředí HPC PowerShell.

3. Spusťte následující příkaz hello:

    ```powershell

    Set-HpcClusterRegistry -SupportAAD true -AADInstance https://login.microsoftonline.com/ -AADAppName HpcClusterServer -AADTenant <your AAD tenant name> -AADClientAppId <client ID> -AADClientAppRedirectUri http://hpcclient
    ```
    kde

    * `AADTenant`Určuje název klienta Azure AD hello, jako například`hpclocal.onmicrosoft.com`
    * `AADClientAppId`Určuje ID hello klienta pro aplikaci hello vytvořili v kroku 2.

4. Restartujte službu HpcSchedulerStateful hello.

    V clusteru s více hlavních uzlech můžete spustit následující příkazy prostředí PowerShell na hello hlavního uzlu tooswitch hello primární repliky pro službu HpcSchedulerStateful hello hello:

    ```powershell
    Connect-ServiceFabricCluster

    Move-ServiceFabricPrimaryReplica –ServiceName “fabric:/HpcApplication/SchedulerStatefulService”

    ```

## <a name="step-4-manage-and-submit-jobs-from-hello-client"></a>Krok 4: Správa a odesílání úloh z klienta hello

tooinstall hello HPC Pack klienta nástroje do počítače stáhnout instalační soubory HPC Pack 2016 (úplná instalace) z hello Microsoft Download Center. Když zahájíte instalaci hello, zvolte možnost instalace hello pro hello **HPC Pack klienta nástroje**.

tooprepare hello klientský počítač, nainstalujte certifikát hello používá během [nastavení clusteru HPC](hpcpack-2016-cluster.md) na klientském počítači hello. Standardní Windows pomocí certifikátu správy postupy tooinstall hello veřejný certifikát toohello **certifikáty – aktuální uživatel** > **důvěryhodné kořenové certifikační autority** uložit. 

Teď můžete spustit příkazy hello HPC Pack nebo použít hello grafického uživatelského rozhraní Správce úloh HPC Pack toosubmit a spravovat úlohy clusteru pomocí účtu hello Azure AD. Možnosti pro odeslání úlohy najdete v tématu [tooan HPC odeslání úlohy HPC Pack clusteru v Azure](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).

> [!NOTE]
> Když zkusíte tooconnect clusteru toohello HPC Pack v Azure pro hello poprvé, zobrazí se místní windows. Zadejte vaše přihlašovací údaje toolog Azure AD v. Hello token se pak uloží do mezipaměti. Novější clusteru toohello připojení v Azure pomocí hello do mezipaměti token, pokud není zaškrtnuté změny ověřování nebo hello do mezipaměti.
>
  
Například po dokončení předchozích kroků hello se můžete dotazovat pro úlohy z lokálního klienta následujícím způsobem:

```powershell 
Get-HpcJob –State All –Scheduler https://<Azure load balancer DNS name> -Owner <Azure AD account>
```

## <a name="useful-cmdlets-for-job-submission-with-azure-ad-integration"></a>Užitečné rutiny pro odeslání úlohy s integrací služby Azure AD 

### <a name="manage-hello-local-token-cache"></a>Spravovat místní mezipaměti tokenu hello

HPC Pack 2016 poskytuje dvě nové rutiny prostředí HPC PowerShell toomanage hello místní tokenu mezipaměti. Tyto rutiny jsou užitečné pro odesílání úloh interaktivně. Viz následující ukázka hello:

```powershell
Remove-HpcTokenCache

$SecurePassword = "<password>" | ConvertTo-SecureString -AsPlainText -Force

Set-HpcTokenCache -UserName <AADUsername> -Password $SecurePassword -scheduler https://<Azure load balancer DNS name> 
```

### <a name="set-hello-credentials-for-submitting-jobs-using-hello-azure-ad-account"></a>Nastavte hello přihlašovací údaje pro odesílání úloh, pomocí účtu hello Azure AD 

V některých případech můžete chtít úlohy hello toorun pod uživatelským clusteru HPC hello (pro připojené k doméně clusteru HPC, spustit jako uživatele domény jeden; pro domény nepřipojená clusteru HPC, spustit jako jeden místní uživatel hello hlavního uzlu).

1. Použijte následující příkazy tooset hello pověření hello:

    ```powershell
    $localUser = “<username>”

    $localUserPassword=”<password>”

    $secpasswd = ConvertTo-SecureString $localUserPassword -AsPlainText -Force

    $mycreds = New-Object System.Management.Automation.PSCredential ($localUser, $secpasswd)

    Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name>
    ```

2. Pak odešlete úlohu hello následujícím způsobem. Hello úkol se spustí v části $localUser hello výpočetních uzlů.

    ```powershell
    $emptycreds = New-Object System.Management.Automation.PSCredential ($localUser, (new-object System.Security.SecureString))
    ...
    $job = New-HpcJob –Scheduler https://<Azure load balancer DNS name>

    Add-HpcTask -Job $job -CommandLine "ping localhost" -Scheduler https://<Azure load balancer DNS name>

    Submit-HpcJob -Job $job -Scheduler https://<Azure load balancer DNS name> -Credential $emptycreds
    ```
    
   Pokud `–Credential` není zadaný s `Submit-HpcJob`, hello úlohy nebo úkolu spouští pod místním uživatelským namapované jako hello účet Azure AD. (clusteru HPC hello vytvoří místního uživatele s hello stejný název jako hello úloh hello toorun účet Azure AD.)
    
3. Nastavit rozšířená data pro hello účet Azure AD. To je užitečné, pokud se používá úlohu MPI na Linuxových uzlů pomocí účtu hello Azure AD.

   * Nastavit rozšířená data pro hello přímo účet Azure AD

      ```powershell
      Set-HpcJobCredential -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data> -AadUser
      ```
      
   * Nastavte Rozšířená data a spusťte jako uživatel clusteru HPC
   
      ```powershell
      Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data>
      ```

