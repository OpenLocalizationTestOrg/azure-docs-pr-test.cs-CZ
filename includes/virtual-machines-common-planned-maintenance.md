Azure pravidelně provádí aktualizace tooimprove hello spolehlivost, výkon a zabezpečení infrastruktury hello hostitele pro virtuální počítače. Tyto aktualizace v rozsahu od opravy softwarové součásti v hello hostitelské prostředí (např. operační systém, hypervisor a různé agenty nasazený na hostiteli hello), upgrade síťových součástí, toohardware vyřazení z provozu. Většina Hello tyto aktualizace se provádějí bez jakékoli dopad toohello hostovaných virtuálních počítačů. Ale existují případy, kdy aktualizace mít vliv:

- Pokud hello údržby nevyžaduje restartování, používá Azure hello toopause místní migrace virtuálních počítačů během hello hostitele je aktualizována.

- Pokud údržby vyžaduje restartování, dostanete oznámení o, když je plánované údržby hello. V těchto případech budete také mít k dispozici časový interval, kde můžete spustit hello údržby sami, v čase, který vám vyhovuje.

Tato stránka popisuje, jak Microsoft Azure provádí oba typy údržby. Další informace o neplánované události (výpadků), najdete v části Správa hello dostupnosti virtuálních počítačů [Windows] (.. / articles/virtual-machines/windows/manage-availability.md) nebo [Linux](../articles/virtual-machines/linux/manage-availability.md).

Aplikace běžící na virtuálním počítači můžete shromáždění informací o budoucích aktualizacích pomocí hello Metadata služby Azure pro [Windows](../articles/virtual-machines/windows/instance-metadata-service.md) nebo [Linux] (.. / articles/virtual-machines/linux/instance-metadata-service.md).

## <a name="in-place-vm-migration"></a>Místní migrace virtuálních počítačů

Když se aktualizace nevyžadují úplné restartování, použije se při migraci za provozu na místě. Během aktualizace hello hello virtuálního počítače je pozastaven pro přibližně 30 sekund, zachování hello paměti RAM, během hello hostování prostředí hello potřebné aktualizace a opravy. potom Hello virtuálního počítače obnoví a automaticky synchronizuje hodiny hello hello virtuálního počítače.

U virtuálních počítačů ve skupinách dostupnosti aktualizace domény jsou aktualizované současně. Všechny virtuální počítače v jedné doméně aktualizace (UD) jsou pozastavena, aktualizovat a potom obnovit před plánovanou údržbu přesune na toohello další UD.

Některé aplikace může být ovlivněno tyto typy aktualizací. Aplikace, které provádějí v reálném čase událostí zpracování, jako jsou datové proudy media nebo překódování nebo vysokou propustnost sítě scénáře, nemusí být navrženou tootolerate 30 sekundové pauzy. <!-- sooooo, what should they do? --> 


## <a name="maintenance-requiring-a-reboot"></a>Údržby, které vyžadují restartování

Pokud virtuální počítače potřebovat toobe restartovat kvůli plánované údržbě, zobrazí se zpráva předem. Plánovaná údržba má dvě fáze: hello okno samoobslužné služby a plánované údržby.

Hello **samoobslužné služby okno** umožňuje zahájení údržby hello na virtuální počítače. Během této doby se můžete dotazovat jednotlivých virtuálních počítačů toosee jejich stav a zkontrolujte výsledek hello vaší poslední žádosti údržby.

Spouštění samoobslužné služby údržby je virtuální počítač přesunutý tooa uzlu, který již byl aktualizován a pak ho znovu zapne. Protože hello virtuální počítač se restartuje, dojde ke ztrátě dočasným diskovým hello a dynamické IP adresy přidružené k rozhraní virtuální sítě se aktualizují.

Pokud spustíte samoobslužné služby údržby a dojde k chybě během procesu hello, operace hello je zastavena, hello, není aktualizován virtuálních počítačů a se odebere i z iterace hello plánované údržby. Můžete se kontaktovat v později pomocí nového plánu a nabízí novou možnost toodo samoobslužné služby údržby. 

Když hello samoobslužné služby okno období uplyne, hello **plánované údržby** začne. Během této doby čas můžete stále dotazu pro hello údržby, ale již nebude možné toostart hello údržby sami.

## <a name="availability-considerations-during-planned-maintenance"></a>Důležité informace o dostupnosti během plánované údržby 

Pokud se rozhodnete toowait dokud hello plánované údržby, existuje pár věcí tooconsider pro údržbu hello nejvyšší availabilty vaše virtuálních počítačů. 

### <a name="paired-regions"></a>Spárované oblastí

Každé oblasti Azure je spárován s jinou oblast v rámci hello stejné geography, společně provádění pár místní. Během plánované údržby bude Azure aktualizovat pouze hello virtuálních počítačů v jedné oblasti pár oblast. Například při aktualizaci hello virtuálních počítačů v Severní jihu USA, Azure nebude aktualizujte všechny virtuální počítače v jihu USA v hello stejnou dobu. Ale jiných oblastech, jako je Severní Evropa může být v rámci údržby v hello stejný čas jako východní USA. Pochopení fungování oblast páry vám může pomoci lépe distribuovat virtuální počítače v oblastech. Další informace najdete v tématu [oblast Azure páry](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

### <a name="availability-sets-and-scale-sets"></a>Skupiny dostupnosti a sady škálování

Pokud nasazujete zatížení na virtuálních počítačích Azure, můžete vytvořit hello virtuálních počítačů v rámci aplikace tooyour vysokou dostupnost tooprovide sady dostupnosti. To zajistí, že během výpadku nebo údržby událostí, je k dispozici alespoň jeden virtuální počítač.

V rámci skupiny dostupnosti jednotlivé virtuální počítače jsou rozloženy do too20 aktualizace domény (UDs). Během plánované údržby je v každém okamžiku dopad pouze jednu aktualizaci domény. Upozorňujeme, že pořadí hello aktualizace domén ovlivněný neodehrává nutně postupně. 

Sady škálování virtuálního počítače se Azure výpočtový prostředek, který vám umožní toodeploy a spravovat sadu identické virtuální počítače jako jeden prostředek. Sada škálování Hello je automaticky nasadí napříč doménami aktualizace, jako jsou virtuální počítače v nastavení dostupnosti. Stejně jako s skupiny dostupnosti s sady škálování pouze jednu aktualizaci doméně ovlivní v každém okamžiku.

Další informace o konfiguraci virtuálních počítačů pro vysokou dostupnost, najdete v části Správa hello dostupnosti virtuálních počítačů pro Windows (.. / articles/virtual-machines/windows/manage-availability.md) nebo [Linux](../articles/virtual-machines/linux/manage-availability.md).
