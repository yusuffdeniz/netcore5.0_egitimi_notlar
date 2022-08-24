
# View Veri Taşıma Kontrolleri

.NET CORE 5.0 Mimarisinde Bulunan Model View arası veri taşıma kontrolleri şunlardır



##  Veri Taşıma Kontrolleri
- #### Model Bazlı Veri Gönderimi
View'a model bazlı veri göndermek istiyorsak ilgili view'ın controllerında geri dönüş olarak return View(modeladi) overload fonksiyonunu kullanmalıyız
```C#
   public ViewResult Index()
        {
        //önceden oluşturulmuş 3 property'e sahip product classımız var
            var products = new List<Product>
            {
                new Product{ Name ="a",stock =1,ID=1},
                new Product{ Name ="b",stock =2,ID=2},
                new Product{ Name ="c",stock =5,ID=3}
            };
         
            return View(products);
        }
```
View Tarafında ise modele şu şekilde erişilir
```CSHTML
@using WebApplication1.Model
@model List<Product>
<html>
<body>
		@{
			foreach(var product in Model)
			{
				<li>@product.Name</li>
			}
		}
	
</body>
</html>
```` 
- #### ViewBag:
View'e gönderilecek datayı dinamik bir tür ile aktarmamızı sağlayan veri taşıma kontrolüdür
```C#
//Controller tarafında ViewBag.DegiskenAdi=Deger Seklinde Tanımlama yapılır.
        public ViewResult Index()
        {

            var products = new List<Product>
            {
                new Product{ Name ="a",stock =1,ID=1},
                new Product{ Name ="b",stock =2,ID=2},
                new Product{ Name ="c",stock =5,ID=3}
            };
            ViewBag.products = products;
            return View();
        }
```` 
View Tarafında ise Veriye şu şekilde erişilir

```CSHTML
@using WebApplication1.Model

<html>
<body>
		@{
			foreach(var product in ViewBag.Products as List<Product>)
			{
				<li>@product.Name</li>
			}
		}
	
</body>
</html>
```

- #### ViewData:ViewBagde olduğu gibi veriyi view aktaran kontroldür ViewBag dinamik olarak çalışırken ViewDAta veriyi Boxing eder dolayısıyla view tarafında veri unboxing ederek kullanabiliriz.
 ```C#
  public ViewResult Index()
        {

            var products = new List<Product>
            {
                new Product{ Name ="a",stock =1,ID=1},
                new Product{ Name ="b",stock =2,ID=2},
                new Product{ Name ="c",stock =5,ID=3}
            };
            ViewData["products"] = products;
            return View();
        }
```
 ```CSHTML
 @using WebApplication1.Model
<html>
<body>
		@{
			foreach(var product in  ViewData["products"] as List<Product>)
			{
				<li>@product.Name</li>
			}
		}
</body>
</html>
 ```

- #### TempData:ViewDate'da olduğu gibi veriyi view'e boxing ile  aktaran kontroldür.Farklı Bir Action'a yönlendirme yapılınca kaybolmaz veriyi cookie kullanarak saklar
 ```C#
         public IActionResult Index()
        {
            TempData["ax"] = 5;
            return RedirectToAction("Index1");

        }
        public ViewResult Index1()
        {
            var x = TempData["ax"];
            return View();
        }
    }
    //Debug ettiğimiz zaman x değişkenine 5 değeri aktarıldığını görebiliriz
 ```
 - #### Tuple: View'a tuple(demet) şeklinde veri göndermek için kullanılır
 
[İlgili Örnek İçin Tıklayın](http://blog.alicancevik.com/mvc-controllerdan-viewe-birden-fazla-model-gonderme/)



  
