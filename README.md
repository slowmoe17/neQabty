# Neqabty API Documentation

## Endpoint
`GET https://community.neqabty.com/api/news?page=1&per_page=4&filter{author.code}=e02`

## Request Method
`GET`

## Allowed Methods
- `GET`
- `POST`

---

## Response Format
```json
{
    "data": {
        "count": 2,
        "next": null,
        "previous": null,
        "results": {
            "news": [
                {
                    "id": 17,
                    "type": "internal",
                    "image": "https://community.neqabty.com/media/news/19_2021-637668956444308727-430.jpeg",
                    "headline": "فتح باب التقديم لتكليف أطباء العلاج الطبيعي",
                    "content": "اعلنت وزاره الصحه عن فتح باب التقديم للتكليف اطباء العلاج الطبيعي قريبا من علي الموقع الرسمي الالكتروني للوزاره",
                    "url": null,
                    "source": "نقابة العلاج الطبيعي",
                    "author": {
                        "id": 15,
                        "name": "نقابة العلاج الطبيعى",
                        "code": "e02",
                        "image": "https://community.neqabty.com/media/Entities/WhatsApp_Image_2023-03-06_at_1.49.14_PM.jpeg"
                    }
                },
                {
                    "id": 11,
                    "type": "internal",
                    "image": "https://community.neqabty.com/media/news/WhatsApp_Image_2023-08-07_at_8.16.30_PM.jpeg",
                    "headline": "بدء منحه جزئيه لاعضاء نقابه العلاج الطبيعي",
                    "content": "اعلنت نقابه العلاج الطبيعي GPTS عن بدء منحه جزئيه لاعضاء نقابه العلاج الطبيعي \n٠دكتوراه اداره الاعمال DBA\n٠ماجيستر اداره الاعمال MBA \n40٪ خصم \nمواعيد الدراسه \nيومين في الاسبوع \nاو يومي الجمعه والسبت",
                    "url": null,
                    "source": "نقابة العلاج الطبيعي",
                    "author": {
                        "id": 15,
                        "name": "نقابة العلاج الطبيعى",
                        "code": "e02",
                        "image": "https://community.neqabty.com/media/Entities/WhatsApp_Image_2023-03-06_at_1.49.14_PM.jpeg"
                    }
                }
            ]
        }
    }
}
```

---

## Model Structure

```python
class News(TimestampedModel):
    """
    News Model
    """

    typeChoices = ("internal", "external")
    author = models.ForeignKey(Entity, verbose_name="المؤلف", on_delete=models.PROTECT)
    headline = models.CharField(max_length=200, verbose_name="عنوان الخبر")
    image = models.ImageField(upload_to="news", blank=True, null=True, verbose_name="الصورة")
    content = models.TextField(verbose_name="المحتوى", null=True, blank=True)
    url = models.URLField(verbose_name="الرابط-URL", max_length=400, blank=True, null=True)
    type = models.CharField(verbose_name="نوع الخبر", choices=typeChoices, default="internal", max_length=20)
    source = models.CharField(max_length=200, verbose_name="المصدر", null=True, blank=True)

    def __str__(self):
        return f"{self.headline} |by: {self.author.name}"

    class Meta:
        verbose_name_plural = "الاخبار"
```

---

## API Field Mapping

| Field    | Type      | Description |
|----------|----------|-------------|
| `id` | `integer` | Unique identifier for the news item |
| `type` | `string` | Type of news (`internal` or `external`) |
| `image` | `string (URL)` | Image associated with the news item |
| `headline` | `string` | Title of the news article |
| `content` | `text` | Main content of the news item |
| `url` | `string (URL)` | Optional link related to the news |
| `source` | `string` | Source of the news item |
| `author` | `object` | Information about the news author |
| `author.id` | `integer` | Author's unique identifier |
| `author.name` | `string` | Name of the author |
| `author.code` | `string` | Unique author code |
| `author.image` | `string (URL)` | Author's profile image |

---

## Filters & Query Parameters

- `page`: Page number for pagination
- `per_page`: Number of news items per page
- `filter{author.code}`: Filter news by author code

---

## Example Request

```bash
curl -X GET "https://community.neqabty.com/api/news?page=1&per_page=4&filter{author.code}=e02" -H "Accept: application/json"
```

## Example Response
```json
{
    "data": {
        "count": 2,
        "results": {
            "news": [
                {
                    "id": 17,
                    "headline": "فتح باب التقديم لتكليف أطباء العلاج الطبيعي",
                    "type": "internal",
                    "source": "نقابة العلاج الطبيعي"
                }
            ]
        }
    }
}
```

