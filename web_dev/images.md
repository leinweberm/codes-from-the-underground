# POUŽITÍ OBRÁZKŮ V HTML

> ### Optimalizace výkonu
>
> - [Lazy Loading](#a-lazy-loading)
> - [Fetch Priority](#b-priorita)
> - [Comulative Layout Shift **CLS**](#c-comulative-layout-shift-cls)
> - [Largest Contentful Paint **LCP**](#d-largest-contentful-paint-lcp)
>
> ### Formáty obrázků
>
> - [WebP formát](#2-formát-obrázku-webp)
> - [AVIF formát](#3-formát-obrázku-avif)
>
> ### Responzivní obrázky
>
> - [Základy responzivních obrázků](#responzivní-obrázky)
> - [`<picture>`](#picture)
> - [Device pixel ratio DPR](#device-pixel-ratio-dpr)
> - [srcset](#srcset)
> - [sizes](#sizes)

<br /><br />

## **1. Optimalizace výkonu**

### **A) Lazy Loading**

```html
<img src="image.jpg" alt="picture" loading="lazy" />
```

Defaultně prohlížece stahují obrázky pro stránku okamžitě při načítání. Pomocí lazy loading můžeme odložit odeslaní requestu pro načtení obrázku do chvíle, kdzy je obrázek skutečně potřeba.<br />

<p style='color:green'>
<b style='font-size: 16px'>+ Plusy</b><br />
	Rychlejší prvotní načtení stránky.<br />
	Neplýtvá se bandwidth na stahování obrázku, které navštěvník webu nevidí.<br />
	Nižší spotřeba mobilních dat na telefonech.
</p>

<p style='color:red'>
<b style='font-size: 16px'>+ Mínusy</b><br />
	Načitání obrázků přes lazy loading může pro návštěvníka působit jako pomalé, jsou-li obrázky umístěny v ploše, kterou návštěvník webu vidí hned po načtení stránky.
</p><br />

### **B) Priorita**

```html
<img src="image.jpg" alt="picture" fetchpriority="low" />
```

Eperimentální attribute, který má pomoci vývojářům lépe kontrolovat prioritu, jakou prohlížeč posílá requesty na stažení podkladů pro váš web. Tento attribut má dvě možnosti low a high.
<br /><br />

### **C) Comulative Layout Shift (CLS)**

CLS = Ukazatel vizuální stabilty webové stránky. Je to metrický ukazatel toho, jak moc se mění layout vaší stránky v průběhu načitání a renderu. Obrázky jsou častou příčinou špatného CLS hodnocení, protože z veškerého obsahu viditelného na stránce jsou to většinou ony, které jsou datově největší a jejich načítání trvá nejdéle.<br/>
Způsob jakým tomu můžeme předcházet je za použití `width` a `height` attributu u obrázku např.

```html
<img width src="image.jpg" height="200" width="400" alt="..." />
```

Tímto způsobem můžeme na lazoutu stránky obrázku orientačně vyhradit místo, a tím se vyhnout příliš velkého vizuálního odskoku layoutu při dokončení načítání obrázku.<br/>
<br/>
Další možností, jak se vyvarovat špatnému CLS hodnocení je použít low quality obrázek, který poslouží jako placeholder do doby, než se načte finální obraz.<br/>

### **D) Largest Contentful Paint (LCP)**

Je ukazatel toho, jak dlouho trvá načtení plošně největšího elementu (viditelného ve viewportu) obsahující nějaký obsah/content. LCP je jeden z klíčových ukazatelů výkonu. Elementy, které nejsou z pohledu uživatele po načtení stránky vidět ve viewportu, nejsou touto metrikou zohledňovány. Na více než 70% stránek je plošně největším elementem právě obrázen na index stránce. Proto pokud máme na index stránce velký obrázek, chceme aby tento obrázek byl načten, co možná nejrychleji. Proto na tento image nechce nikdy použít `loading="lazy"` a naopak chceme v ideálním případě použít `fetchpriority="high"`. A samozřejmě optimalizace zdrojového obrázku jako přiměřená datová velikost a vhodně vybranný formát jsou taktéž velice důležité.<br/>
<br/>

## **2. Formát obrázku: WebP**

Versatilní formát obrázku, který při srovnatelné kvalitě nabízí menší datovou velikost než například JPEG. Zároveň podporuje alpha channel průhlednost jako PNG a GIFu podobné animace. Kvalitu obrázku a míru komprese kontrolujeme stejně jako u JPEG jedním ukazatel kvality 0-100%. Nicméně při používání WebP je třeba počítat s tím, že výsledný obrázek se může po kompresi barevně trochu lyšit a může vyžadovat drobné doladění buď prostřednictvím úpravy encodingu WebP obrázku v Command Line, nebo úpravou v některém z grafických editorů.<br/>
<br/>

## **3. Formát obrázku: AVIF**

Ještě novější formát než WebP a stejně jako on nabízí menší velikost než srovnatelně kvalitní JPEG i WebP, GIFu podobné animace a podporu průhlednosti jako PNG. Nicméně je podporován pouze novými prohlížeči a to: Chrome od roku 2020, Opera od roku 2020, FireFox 2021 a Safari 2022.<br/>
Tento formát upoutal pozornost například Netflixu, který jej intenzivně testuje a podle výsledků testů se zatím zdá, že bude má tento formát obrovský potenciál. I když editování AVIF souborů je zatím dost omezené, můžeme využít jedno z encodování nabízených Squoosh. Tento formát ovšem není na rozdíl od WebP standartně podporován všemi novými prohlížeči.
<br/><br/>

## **4. Responzivní obrázky**

Responzivn9 obrázek můžeme chtít použit ve dvou případech. Buď protože potřebujeme vybrat co nejefektivněji, jaký obrázek prohlížeč načte. Nebo protože potřebujeme mít explicitní kontrolu nad tím jaký zdroj (src) prohlížeč použije.<br/>
Použitím responsivního obrázku umožňujeme prohlížeči vybrat nejvhodnější zdroj obrázku na základě informací, které má uživateli a možnostech, které mu dáme.

### **`<picture>`**

Použijeme ve chvíli, kdy potřebujeme mít explicitní kontrolu nad obrázkem, takovou, které bychom nedosáhli při použití **`<image>`**. Můžeme tím ovlivnit například to, jaký obrázek se má načíst na základě námi nadefinových pravidel případě změn layoutu webu vyvolaných například breakpointy v našich stylech. Můžeme tímto také kontrolovat i vlatnosti načteného obrázku jako třeba aspect-ratio atd...

### **Device pixel ratio (DPR)**

Je to poměr mezi logickým a fyzickým rozlišením displeje. Vypočítá se vydělením viewport CSS pixelů reálným rozlišením displeje. První zařízení s DPR větším než 1 byl iPhone 4. Dnešní telefony mají běžné DPR v rozmezí 2.5 - 4.<br/>
Pokud zobrazíme obrázek široký 400px na zařízení s DPR 2, tak dojde k upscallingu a každý 1px obrázku zabere 4px na displeji zařízen. 2 horizontálně a 2 veritkálně. Pomocí **`srcset`** můžeme zajistit, aby jim prohlížeč naservíroval obrázek s vyšším rozlišením, který na jejich displeji bude vypadat dobře. A zárove_n nebude prohlížeč servírovat zbytečně velké high resolution obrázky zařízením s nízkým DPR a tím plýtvat jejich bandwidthem.

### **`srcset`**

Poskytuje prohlížeči seznam možností, které může použít jako zdroj pro obrázek. Prohlížeč v tomto případě zohledňuje aspekty jako velikost viewportu uživatele, rozlišení displeje, uživatelské nastavení prohlížeče, bandwidth a další.<br/>
srcset attribute je list možností oddělených čárkou skladajících se ze dvou parametrů. Tím prvním je url obrázku, a syntaxi popisujícím daný obrázek (buď šířka nebo požadovaná densita obrázku).<br/>
**_Příkla dole dává prohlížeči na výběr ze tří variant obrázků pro displeje s DPR 1, 2 a 3._**

```html
<img
	src="low_density.jpg"
	srcset="double_density.jpg 2x, tripple_density.jpg 3x"
	alt="..."
/>
```

**_Příklad použítí srcset s parametrem šířky obrázku namísto DPR_**

```html
<img
	src="low_density.jpg"
	srcset="small.jpeg 600w, large.jpg 1200w"
	alt="..."
/>
```

Můžeme také zohlednit pokud má uživatel zapnuté šetření dat a to použítím **`prefers-reduced-data`** v media query. Potom bude prohlížeč vždy dávat přednost obrázku s menší datovou velikostí. Nicméně při použití **`srcset`** a **`sizes`** dáváme prohlížeči možnost dělat tuto optimalizaci automaticky, pokud to vyhodnotí jako žádoucí.

### **`sizes`**

Request pro načtení obrázků posílá prohlížeč okamžitě potom, co obdrží od web serveru markdown našeho webu. Tento request je tedy posílán už ve chvíli, kdy prohlížeč nemá žádné informace z našich stylesheetů nebo javascript souborů. V tu chvíli ma prohlížeš k dispozici pouze informace o velikosti viewportu uživatele,
