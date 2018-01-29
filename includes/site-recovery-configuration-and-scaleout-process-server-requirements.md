| **Komponenta** | **Požadavek** |
| --- |---|
| Procesorová jádra| 8 |
| Paměť RAM | 12 GB|
| Počet disků | 3, včetně disku operačního systému, proces disku mezipaměti serveru a jednotka pro uchování pro navrácení služeb po obnovení |
| Volné místo na disku (mezipaměť procesového serveru) | 600 GB
| Volné místo na disku (disk pro uchování) | 600 GB|
| Operační systém  | Windows Server 2012 R2 <br> Windows Server 2016 |
| Národní prostředí operačního systému | Angličtina (en-us)|
| Verze VMware vSphere PowerCLI | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0")|
| Role Windows Serveru | Nepovolíte tyto role: <br> – Active Directory Domain Services <br>– Internet Information Service <br> – Hyper-V |
| Zásady skupiny| Nemáte povolit tyto zásady skupiny: <br> -Zabránit přístupu do příkazového řádku <br> -Zabránit přístupu do nástroje pro úpravy registru <br> -Důvěřovat logiku pro přílohy souborů <br> -Zapnout provádění skriptu <br> [Další informace](https://technet.microsoft.com/en-us/library/gg176671(v=ws.10).aspx)|
| IIS | -Žádné existující výchozí web <br> -Aktivovat [anonymní ověřování](https://technet.microsoft.com/en-us/library/cc731244(v=ws.10).aspx) <br> -Aktivovat [FastCGI](https://technet.microsoft.com/en-us/library/cc753077(v=ws.10).aspx) nastavení  <br> -Žádná existující web nebo aplikace naslouchá na portu 443<br>|
| Typ síťový adaptér | VMXNET3 (Pokud je nasazený jako virtuální počítač VMware) |
| Typ IP adresy | Statická |
| Přístup k internetu | Server potřebuje přístup k tyto adresy URL: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (nevyžaduje se pro procesový server pro horizontální navýšení kapacity) <br> - time.nist.gov <br> - time.windows.com |
| Porty | 443 (orchestrace řídicího kanálu)<br>9443 (přenos dat)|
