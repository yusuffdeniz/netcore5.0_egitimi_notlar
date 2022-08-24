
# Kullanıcıdan Veri Alma Yöntemleri

## Form Üzerinden Veri Alma
- ### IFormCollection Kullanarak Veri Alma:
View Tarafında form inputlarına verilen isimle controller tarafında IformCollection[inputismi] kullanılarak erişilir
Controller
```C#
        [HttpPost]
        public IActionResult CreateProduct(IFormCollection datas)
        {
            var stock = datas["stock"];
            var name = datas["name"];
            return View();
        }
```
View
```CSHTML
@addTagHelper * ,Microsoft.AspNetCore.Mvc.TagHelpers

<form asp-action="CreateProduct" method="post">

	<input type="text" placeholder="Ad" name="Name" />
	<br />
	<input type="number" placeholder="Quantity" name="stock" />
	<br />
	<button>Ekle</button>
</form>
```

- ### Fonksiyon Parametreleri Kullanarak Alma
Form inputlarının ismiyle fonksiyonumuza parametre eklersek otomatik olarak veri bindlanmış olur
```C#
        public IActionResult CreateProduct(string name, int stock)
        {
            var _stock = stock;
            var _name = name;
            return View();
        }
 ```

```CSHTML
@addTagHelper * ,Microsoft.AspNetCore.Mvc.TagHelpers

<form asp-action="CreateProduct" method="post">

	<input type="text" placeholder="Ad" name="Name" />
	<br />
	<input type="number" placeholder="Quantity" name="stock" />
	<br />
	<button>Ekle</button>
</form>
```
- ### Bir Class Nesnesi Kullanarak Veri Alma
Oluşturduğumuz classtaki propertyler ile formdaki inputların nameleri aynı olmalıdır böylelike veriyi oluşturduğumuz instance'dan alabiliriz.view sayfasında model belirtilmelidir

Classımızı Tanımlayalım
```c#
namespace WebApplication1.Model
{
    public class Product
    {
        public int stock { get; set; }
        public string Name { get; set; }

    }

}
```
VIEW
```CSHTML
@addTagHelper * ,Microsoft.AspNetCore.Mvc.TagHelpers
@model WebApplication1.Model.Product

<form asp-action="CreateProduct" method="post">

	<input type="text" placeholder="Ad" name="Name" />
	<br />
	<input type="number" placeholder="Quantity" name="stock" />
	<br />
	<button>Ekle</button>
</form>
```
```C#
        [HttpPost]
        public IActionResult CreateProduct(Product p)
        {
            var _stock = p.stock;
            var _name = p.Name;
            return View();
        }
```
- ## Query String Kullanarak Veri Alma
Güvenlik gerektirmeyen verilerin URL üzerinden taşınmasını sağlar.Query string yapılan requestin her ne olursa olsun taşınabilir get/post vs.
localhost:44347/Home/CreateProduct?a=5&b=2 gibi gelen bir istegi controllerda yakalamak icin
```C#

        public IActionResult CreateProduct()
        {
            var querya = Request.Query["a"].ToString();
            var queryb = Request.Query["b"].ToString();
            return View();
        }
```
 - ## Header Üzerinden Veri Alma
 ```C#
 
        public IActionResult CreateProduct()
        {
            var headers = Request.Headers["header_ismi"];

            return View();
        }
 ```
- ## Ajax İle Client Tabanlı Veri Alma

- ## Tuple İle Veri Alma





