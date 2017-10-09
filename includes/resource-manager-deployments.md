## <a name="incremental-and-complete-deployments"></a>Přírůstkové a úplné nasazení
Při nasazení vašich prostředků, zadejte, že hello nasazení je k přírůstkové aktualizaci nebo kompletní aktualizace. Hello hlavní rozdíl mezi tyto dva režimy je, jak Resource Manager zpracovává existující prostředky ve skupině prostředků hello, které nejsou v šabloně hello:

* V dokončení režimu Resource Manager **odstraní** prostředky, které existují ve skupině prostředků hello, ale nejsou zadané v šabloně hello. 
* V přírůstkové režimu Resource Manager **zůstane beze změny** prostředky, které existují ve skupině prostředků hello, ale nejsou zadané v šabloně hello.

Pro oba režimy Resource Manager pokusí tooprovision všechny prostředky zadané v šabloně hello. Pokud už existuje prostředek hello ve skupině prostředků hello a jsou stejné jako jeho nastavení, výsledkem operace hello žádná změna. Pokud změníte nastavení hello prostředků, prostředků hello je opatřen tyto nové nastavení. Když zkusíte tooupdate hello umístění nebo typ existující prostředek, hello nasazení se nezdaří s chybou. Místo toho nasaďte nový prostředek s umístěním hello nebo typu, je nutné.

Ve výchozím nastavení používá hello přírůstkové režimu Resource Manager.

tooillustrate hello rozdíl mezi režimy přírůstkové a úplné, zvažte následující scénáře hello.

**Existující skupinu prostředků** obsahuje:

* Prostředek A
* Prostředek B
* Prostředek C

**Šablona** definuje:

* Prostředek A
* Prostředek B
* Prostředek D

Při nasazení v **přírůstkové** režimu hello skupina prostředků obsahuje:

* Prostředek A
* Prostředek B
* Prostředek C
* Prostředek D

Při nasazení v **dokončení** režimu C prostředků se odstraní. Skupina prostředků Hello obsahuje:

* Prostředek A
* Prostředek B
* Prostředek D
