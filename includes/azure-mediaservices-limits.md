>[!NOTE]
>Pro prostředky, které nejsou opravit může požádat o toobe kvóty hello vyvolá otevřením lístku podpory. Proveďte **není** vytvořit další účty Azure Media Services ve snaze tooobtain vyšší limity.

| Prostředek | Výchozí omezení | 
| --- | --- | 
| Účty Azure Media Services (AMS) v rámci jednoho předplatného | 25 (pevné) |
| Rezervované jednotky médií (RU) na jeden účet AMS |25 (S1, S2)<br/>10 (S3) <sup>(1)</sup> | 
| Úlohy na jeden účet AMS | 50 000<sup>(2)</sup> |
| Zřetězené úkoly na jednu úlohu | 30 (pevné) |
| Prostředky na jeden účet AMS | 1 000 000|
| Prostředky na jeden úkol | 50 |
| Prostředky na jednu úlohu | 100 |
| Jedinečné lokátory přidružené k prostředku najednou | 5<sup>(4)</sup> |
| Živé kanály na jeden účet AMS |5|
| Programy v zastaveném stavu na jeden kanál |50|
| Programy ve spuštěném stavu na jeden kanál |3|
| Koncové body streamování ve spuštěném stavu na jeden účet AMS|2|
| Jednotky streamování na jeden koncový bod streamování |10 |
| Účty úložiště | 1 000<sup>(5)</sup> (pevné) |
| Zásady | 1 000 000<sup>(6)</sup> |
| Velikost souboru| V některých případech je limit na hello maximální podporovaná velikost souboru ke zpracování ve službě Media Services. <sup>7</sup> |
  
<sup>1</sup> Jednotky RU S3 nejsou dostupné v oblasti Indie – západ. maximální omezení RU Hello získat obnovit, pokud zákazník hello změní hello typu (například z S2 tooS1). 

<sup>2</sup> Tato hodnota zahrnuje úlohy zařazené do fronty a dokončené, aktivní a zrušené úlohy. Nezahrnuje odstraněné úlohy. Můžete odstranit staré úloh hello pomocí **IJob.Delete** nebo hello **odstranit** požadavek HTTP.

Od 1. dubna 2017 záznam všechny úlohy ve vašem účtu, který je starší než 90 dní se automaticky odstraní, společně s jeho přidružené záznamy úloh i v případě, že hello celkový počet záznamů je nižší než maximální kvóty hello. Pokud potřebujete tooarchive hello úloh informací, můžete použít kód hello popsané [zde](../articles/media-services/media-services-dotnet-manage-entities.md).

<sup>3</sup> při vytváření požadavku toolist úlohy entity, maximálně 1 000 vrátí na žádost. Pokud je třeba tookeep zaznamenávat všechny odeslání úlohy, můžete použít horní/skip, jak je popsáno v [možností dotazu systému OData](http://msdn.microsoft.com/library/gg309461.aspx).

<sup>4</sup> Lokátory nejsou určené ke správě řízení přístupu pro jednotlivé uživatele. Uživatelé s právy tooindividual toogive různý přístup, použijte řešení pro správu digitálních práv (DRM). Další informace najdete v [tomto](../articles/media-services/media-services-content-protection-overview.md) oddílu.

<sup>5</sup> hello účtů úložiště musí být z hello stejného předplatného Azure.

<sup>6</sup> Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy). 

>[!NOTE]
> Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění / atd. Informace a příklad najdete v [tomto](../articles/media-services/media-services-dotnet-manage-entities.md#limit-access-policies) oddílu.

<sup>7</sup>Pokud nahráváte obsahu tooan Asset v Azure Media Services s úmyslem tooprocess hello ji můžete jedním hello procesory médií v naší službě (tj. kodéry jako Media Encoder Standard a pracovní postup Premium kodér médií nebo analysis moduly jako vzhled detektor) pak byste měli být vědomi hello omezení maximální velikosti hello. 

Od 15. května 2017 hello maximální velikost podporovaná pro jeden objekt blob je 195 TB - s largers souboru než tento limit, nebude možné úlohu. Oprava tooaddress pracujeme toto omezení. Kromě toho hello omezení maximální velikosti hello hello Asset je následující.

| Typ rezervovaných jednotek médií | Maximální velikost vstupní (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
