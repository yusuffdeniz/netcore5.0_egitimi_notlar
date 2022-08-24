
# Action Types ve NonAction




Client Tarafından gelen isteklere karşılık Controller Classlarımızın Vereceği Cevapları Döndüren Metotlara Action Methods Denir.\
### .NET CORE 5.0 da kullanılan Action Metodları Şunlardır:

- ViewResult:
Kendi isminde Views klasörü altında bulunan .cshtml Viewini Döndürür 

```C#
public ViewResult GetProducts()
{
return View();
}
```
- PartialViewResult:
Yine View döndürür ancak ajax sorguları kullanmaya yatkındır sayfanın tamamını değil(yani viewstartı değil) sayfanın değişen parçasını döndürür.
```C#
public PartialViewResult GetProducts()
{
PartialViewResult result=PartialView();
return result;
}
```
- JsonResult:
Üretilen datayı JSON türüne dönüştürüp döndüren action türüdür
```C#
public JsonResult GetProducts()
{
JsonResult result=Json(new Product{
ID=1,
quantity=5
});
return result;
}
```
- EmptyResult:
Bazen Gelen isteklere herhangi bir şey döndürmek istemeyiz böyle bir durumda EmptyResult Action metodu kullanabiliriz.
```C#
public EmptyResult GetProducts()
{

return new EmptyResult();
}
```
- ContentResult:
İsteğe Metin Formatında Response veren action türüdür.
```C#
public EmptyResult GetProducts()
{
ContentResult result= Content("Hello From Content Result");
return result;
}
```
- ActionResult: 
Gelen isteğe verilen cevabın farklılık göstereceği durumlarda kullanılan action türüdür diğer tüm actionların base classıdır.
```C#
public ActionResult GetProducts()
{
if(true)
 return Json(new Object());
else
return Content("sadasdadaasd");
}
```

### NonController-NonAction
Controller Classlarında tanımlanan ancak dışardan istek almamaması,gerektiği zaman diğer actionlar tarafından çağrılması için kullanılan metodlara eklenen özellikdir.
```C#
[NonAction]
public void X()
{

}
```
```bash
 Not: Bir Controller Classının Tamamının Dışardan İstek Almasını Engellemek İstiyorsak Controller Classının tanımlandığı satırının üstüne [NonController] Attribute Ekleyebiliriz
```
Kaynak: \
Özel Ders Formatında A'dan Z'ye Asp.NET Core 5.0  -Gençay Yıldız


  
