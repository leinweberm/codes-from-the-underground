# POUŽITÍ OBRÁZKŮ V HTML
## 1. Optimalizace výkonu
### **a) Lazy Loading**
```html
	<img src="image.jpg" alt="picture" loading="lazy">
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

### **b) Priorita**
```html
	<img src="image.jpg" alt="picture" fetchpriority="low">
```
Eperimentální attribute, který má pomoci vývojářům lépe kontrolovat prioritu, jakou prohlížeč posílá requesty na stažení podkladů pro váš web. Tento attribut má dvě možnosti low a high.
<br /><br />

### **c) Comulative Layout Shift (CLS)**
Je to metrický ukazatel toho, jak moc se mění layout vaší stránky v průběhu načitání a renderu. 


