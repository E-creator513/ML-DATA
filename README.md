# Проект аннотации отзывов TripAdvisor о гостиницах

Этот репозиторий содержит небольшой набор отзывов TripAdvisor о гостиницах и полный рабочий процесс для **ручной аннотации, проверки и анализа**. Цель — создать размеченные данные для **анализa тональности** и **распознавания сущностей** в отзывах о гостиницах.

---

## **1. Набор данных**

Набор данных состоит из отзывов о гостиницах со следующими колонками:

| Колонка  | Описание |
|---------|-------------|
| `Review` | Текст отзыва |
| `Rating` | Оценка от 1 до 5 |

**Пример строк:**

| Review | Rating |
|--------|--------|
| "nice hotel expensive parking got good deal stay hotel anniversary..." | 4 |
| "ok nothing special charge diamond member hilton decided chain shot..." | 2 |

---

## **2. Техническое задание (TOR) для аннотации**

### **A. Тональность отзыва (Review-Level Sentiment)**
- `Positive` → в целом положительный отзыв (рейтинг 4–5)  
- `Neutral` → смешанные или неясные впечатления (рейтинг 3)  
- `Negative` → явно отрицательный отзыв (рейтинг 1–2)  

### **B. Сущности (Entity, выделение текста)**

| Тип сущности       | Что выделять                                     | Пример |
|-------------------|-------------------------------------------------|---------|
| **Hotel Feature**   | Комнаты, удобства, элементы гостиницы           | “jacuzzi tub”, “room clean” |
| **Service**         | Персонал, регистрация, консьерж, уборка        | “desk clerk told”, “housekeeping staff” |
| **User Emotion**    | Эмоции и мнения автора                          | “disappointed”, “love” |
| **Problem/Issue**   | Жалобы или негативный опыт                      | “AC malfunctioned”, “no champagne strawberries” |
| **Positive Highlight** | Позитивные моменты, не связанные напрямую с тональностью | “friendly staff”, “nice gesture” |

**Пример аннотации:**

**Отзыв:**  
> “Room was clean, staff friendly, but AC didn’t work.”  

**Аннотация:**  
- Тональность: Neutral  
- Сущности: `Hotel Feature: "Room"; Service: "staff"; Problem/Issue: "AC didn’t work"; User Emotion: "friendly"`

---

## **3. Валидатор / Проверка согласованности**

- Дайте **2–3 отзыва** **двум аннотаторам**.  
- Метрики:  


```python
from sklearn.metrics import cohen_kappa_score

sentiment_annotator1 = ['Positive','Negative','Neutral']
sentiment_annotator2 = ['Positive','Negative','Positive']

kappa = cohen_kappa_score(sentiment_annotator1, sentiment_annotator2)
print("Cohen's Kappa:", kappa)
