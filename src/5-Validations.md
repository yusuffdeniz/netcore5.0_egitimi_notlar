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
İkinci olarak startup.cs 'den fluent validation servisini projeye entegre etmeliyiz
```C#
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddRazorPages();
            services.AddControllersWithViews().AddFluentValidation(x=> x.RegisterValidatorsFromAssemblyContaining<Startup>());
// STARTUP CLASSININ BULUNDUĞU DİZİNDEKİ TÜM VALİDASYON CLASSLARINI EKLEMEYE YARAR
        }
```
sonraki adımda Model classı altında Validators adlı bir klasör acıp tüm validation classlarını içine ekleyebiliriz

Örnek Bir Validator classı valdiation kurallarımızı classımızın constructor metodunda rulefor fonksiyonuyla aşağıdaki gibi tanımlarız
```c#
using FluentValidation;

namespace WebApplication1.Model.Validators
{
	public class ProductValidator:AbstractValidator<Product>
	{
		public ProductValidator()
		{
			RuleFor(x=> x.Name.Length).NotEmpty().WithMessage("İsim Boş Bırakılamaz");
		}

	}
}

```
startup.cs classımızdan validation classlarımızı ekleyelim
```c#
		public void ConfigureServices(IServiceCollection services)
		{
			services.AddRazorPages();
			services.AddValidatorsFromAssemblyContaining<ProductValidator>();



		}

```
View'da hiç bir değişiklik yapmadan aynı şekilde kullanabiliriz.
```CSHTML
@model WebApplication1.Model.Product
@addTagHelper * ,Microsoft.AspNetCore.Mvc.TagHelpers


<form asp-action="CreateProduct" asp-controller="Home"> 
	<input type="text" asp-for="Name" /> <span asp-validation-for="Name"></span> <br />
	<input type="number" asp-for="stock" /> <span asp-validation-for="stock"></span> <br />
	<button>gönder</button>
</form>
```

Şuana Kadar Yazdığımız Tüm Validasyonlar server tarafı içinde server tarafında yapılan validasyonları aktarmayı ise şöyle yapıyoruz

- ### Server Tarafındaki Validasyonları Client'a aktarma
Bu işlemi gerçekleştirmek için wwwroot dizinimize client side library kullanarak şunları indirmeliyiz
- jquery
- jquery-validate
- jquery-validation-unobtrusive
bu kütüphaneleri yükledikten sonra client tabanlı validation yaptıgımız gerekli view dosyasında tanımlamamız yeterlidir!
```cshtml
@model WebApplication1.Model.Product
@addTagHelper * ,Microsoft.AspNetCore.Mvc.TagHelpers

<script src="~/jquery/jquery.min.js"></script>
<script src="~/jquery-validate/jquery.validate.min.js"></script>
<script src=~/jquery-validation-unobtrusive/jquery.validate.unobtrusive.min.js></script>


<form asp-action="CreateProduct" asp-controller="Home"> 
	<input type="text" asp-for="Name" /> <span asp-validation-for="Name"></span> <br />
	<input type="number" asp-for="stock" /> <span asp-validation-for="stock"></span> <br />
	<button>gönder</button>
</form>
```
