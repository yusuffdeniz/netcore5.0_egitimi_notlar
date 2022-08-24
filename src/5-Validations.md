# Validations

Kullanıcı Tarafından girilen bir değerin server tarafında(Server Side Validation) veya client tarafında(Client Side Validaiton) kontrol edilmesine validation denir.
Validation Paralel bir şekilde client ve server taraflarında uygulanmalıdır.
Server Tarafında model propertylerine verilecek Annotationlar ile Validation yapılabilir.
Örnek
```C#
    public class Product
    {
        [Required(ErrorMessage ="Lütfen Alanı Boş Bırakmayınız!")]
        public int stock { get; set; }
        [StringLength(100,ErrorMessage ="50 Karakterden Fazla Olamaz!")]
        public string Name { get; set; }

    }
```
DataAnnotationsları ayarladıktan sonra Controller kısmında kullanıcının girdiği verilerin validasyonununu şöyle kontrol edebiliriz.
```C#
	[HttpPost]
		public IActionResult CreateProduct(Product p)

		{
			if (!ModelState.IsValid)
			{
				ViewBag.Hata = ModelState.Values.FirstOrDefault(x => x.ValidationState == Microsoft.AspNetCore.Mvc.ModelBinding.ModelValidationState.Invalid).Errors[0].ErrorMessage;

			}

			//veri gecerliyse yapılacak islemler
			return View();
		}
```

veya view kısmında span oluşturarak taghelper ile validation hata mesajını gösterebiliriz

```CSHTML
@model WebApplication1.Model.Product
@addTagHelper * ,Microsoft.AspNetCore.Mvc.TagHelpers


<form asp-action="CreateProduct" asp-controller="Home"> 
	<input type="text" asp-for="Name" /> <span asp-validation-for="Name"></span> <br />
	<input type="number" asp-for="stock" /> <span asp-validation-for="stock"></span> <br />
	<button>gönder</button>
</form>
```

- Data Anotations Çoğunluklla EntityModel ve ViewModel Classlarında kullanılması tavsiye edilmez.Bunun yerine MetaData Classları veya Fluent Validation Kütüphanesi Kullanılır.
- ### Meta Data Classları
Kullanımı Şöyledir Gerekli Varlığımınız Validasyona sahip fiedlarını MetaData classımızda tanımlarız
```C#
using System.ComponentModel.DataAnnotations;

namespace WebApplication1.Model
{
	public class ProductMetaData
	{
		[Required]
		public int stock { get; set; }
		[StringLength(100)]
		public string Name { get; set; }
	}
}
```
Sonraki Adımda Entity Clasımızda meta data classımızı type of ile belirleriz

```c#
using Microsoft.AspNetCore.Mvc;
using System.ComponentModel.DataAnnotations;

namespace WebApplication1.Model
{
	[ModelMetadataType(typeof(ProductMetaData))]
	public class Product
	{
		public int stock { get; set; }
		public string Name { get; set; }

	}

}

```
### Fluent Valdiation Kütüphanesi
İlk olarak nuget Package Manager kullanarak kütüphaneyi projeye dahil etmeliyiz
```bash
Install-Package FluentValidation
```

