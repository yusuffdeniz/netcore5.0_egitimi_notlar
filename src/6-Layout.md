# Layout

Asp.net WebForms' daki MasterPage sayfasının MVC' deki karşılığı Layout sayfalarıdır. Layout sayfalar, web uygulamamızdaki sayfaların iskeleti gibidir. Tüm sayfalarımızda yer alacak olan örneğin üst menü gibi, site logosu gibi kısımların (sürekli tekrar eden) tek bir yerden ve çok daha kolay yönetilmesini sağlar.

- RenderBody()
MVC layout sayfamızda değişken olacak, yani diğer sayfalardan gelecek olan içeriği sayfaya yerleştirmek için @RenderBody() kullanılır.
- RenderSection()
_Layout.cshtml
```CSHTML
< html >
< body >
@RenderBody()
@RenderSection("slider_scripts", required: false)
< / body >
< /html >
```

Index.cshtml
```CSHTML
@section slider_scripts{
< script type ="text/javascript" >alert('Merhaba!');< /script >
}
```
- ViewStart
Ana Layout.cs belirlemek için kullanılan class dosyasıdır 
- Viewİmport
Viewlara kütüphane ve namespaceleri tek tek eklemektense bu dosyaya eklersen tüm viewlardan gerekli alanlara erişebiliriz.

- PartialView
Tasarımda modülerliği sağlar örneğin hepsiburada ana sayfasında üstte bulunan sliderın ayrı bir view dosyasında tasarlandığını alttaki ürünler listesinin farklı bir viewda tasarlandığını düşünebiliriz. kodlarımızın daha düzenli olmasını sağlar

- ViewComponent
PartialView ile aynı işlevi görür ama modelden veri alması gerektiği zaman direkt modelle bağlantı kurar partial view'da ise partial view controllere istek yapar controller modelden aldığı view'ı partial viewe gönderir bu da birden çok partial view kullanılan sayfalarda controllerların meşgul edilmesine sebebiyet verir