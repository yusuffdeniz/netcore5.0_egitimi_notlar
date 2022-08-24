
# Helpers
.Net Core de geliştiricilere kolaylık sağlamak için kullanılan yardımcı metodlara Helper Denir.
### .NET CORE 5.0 da Bulunan Helper Classları Şunlardır:

- #### UrlHelper:
Url Oluşturmak için yardımcı metotlar içeren ve o anki url'e ait bilgileri veren bir classtır.
Startup.cs de belirlene rotaya uygun şekilde oluşturur
```C#
//Action metodu belli kontrol altındaki belli action için url oluşturur
Url.Action("index","product","5");
/*Çıktı olarak
product/index/5 döndürür
*/

 Url.ActionLink("index","product","5");
 /*
 Url.Action gibi kullanılır ancak
 Çıktı Olarak Temel host protokol ve domain bilgisini de ekler
 https://www.domainadi.com/product/index/5 
 */
 Url.RouteUrl("rotaismi");
 /*
 Mimaride Startup.cs dosyası altında belirlediğimiz routelara göre link oluşturur
 */
   Url.RouteUrl("default");
   //Varsayılan Rotaya Göre Link Oluşturur

```
- #### HtmlHelper:
Html Elementlerini server tabanlı oluşturmamızı sağlayan yardımcıdır.Performans Nedeniyle Pek Tercih edilmez.Kendi HtmlHelper Metotlarınızı da ekleyebeilirsiniz
Kullanımı İçin Bkz:
https://www.tutorialsteacher.com/mvc/html-helpers

- #### TagHelper:
Tag Helpers daha okunabilir ve kolay geliştirilebilir bir view inşa etmemizi sağlayan asp.net core ile birlikte htmlheler metodlarının yerine gelen yapıdır.
View'de TagHelper Entegre Etmek İçin
```CSHTML
@addTagHelper * ,Microsoft.AspNetCore.Mvc.TagHelpers
```
Kullanımı İçin Bkz:
https://keremozer.com/aspnet-core/asp-net-core-tag-helper.html