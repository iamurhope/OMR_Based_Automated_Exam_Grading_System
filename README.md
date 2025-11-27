<div align="center">

# OMR Based Automated Exam Grading System
<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" /> <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white" /> <img src="https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white" /> <img src="https://img.shields.io/badge/Computer_Vision-FF6F00?style=for-the-badge&logo=visualstudiocode&logoColor=white" />

Компьютер хараа ашиглан тестийн хуудсыг автоматаар таньж, засах цогц систем. Тестийн хуудсыг тэгшлэх, хариулт таних, оноо тооцоолох, хувилбар шалгах зэрэг үйлдлүүдийг бүрэн автоматжуулсан.

[Ажиллах зарчим](#How-it-works) • [Онцлог](#features) • [Үр дүн](#result)

---

</div>

## Онцлог шинж чанарууд

<table>
<tr>
<td width="50%">

### Автомат Боловсруулалт
- **Ultra-Robust Paper Detection** - 9 өөр edge detection стратеги
- **Perspective Transformation** - Дөрвөн цэгийн авто тэгшилгээ
- **Auto-Rotation Detection** - 180° эргүүлэх шаардлагыг илрүүлнэ
- **CLAHE Enhancement** - Гэрэл тохируулга
- **Adaptive Thresholding** - Янз бүрийн гэрэлтүүлэгт тохирно

</td>
<td width="50%">

### Ухаалаг Илрүүлэлт
- **50 асуултын дэмжлэг** - 25x2 багана загвар
- **Variant Validation** - A, B, C, D, E хувилбарын авто шалгалт
- **Bubble Detection** - Circularity & intensity шинжилгээ
- **Grid Organization** - Y-threshold бүлэглэх алгоритм
- **Confidence Scoring** - Итгэлцлийн түвшин хэмжих

</td>
</tr>
<tr>
<td width="50%">

### Автомат Засалт
- **Answer Key Processing** - Хариултын загвар боловсруулах
- **Batch Grading** - 10 хүртэл оюутныг нэгэн зэрэг засах
- **Variant Matching** - Зөв хувилбарыг шалгаж засах
- **Visual Feedback** - Зөв/буруу харуулсан визуал үүсгэх
- **Detailed Reports** - TXT & JSON тайлан гаргах

</td>
<td width="50%">

### Дэлгэрэнгүй Тайлан
- **Score Statistics** - Дундаж, медиан, std deviation
- **Master Comparison Grid** - 3x4 харьцуулалтын график
- **Per-Question Analysis** - Асуулт бүрийн дэлгэрэнгүй
- **Error Detection** - Хувилбар зөрүү илрүүлэлт
- **JSON Export** - Програмаар ашиглах боломжтой

</td>
</tr>
</table>

---

## How it works

1. **Шалгалтын хуудас оруулах**  
   Систем нь шалгалтын хуудасны зургийг (жишээ: `1-10.jpg`) авч ажиллана.

2. **Хуудас илрүүлэх**  
   Зургийн доторх хуудасыг олох олон стратеги ашиглана.

3. **Перспектив засварлах**  
   Хуудас шулуун, тэгш харагдах байдлаар засварлана.

4. **Чиглэл шалгах**  
   Хуудас буруу эргэлттэй байвал зөв эргүүлэх.

5. **Бүсийг ялгах**  
   Асуулт болон хувилбар бүсүүдийг тодорхойлно.

6. **Бөмбөлөг/хариу илрүүлэх**  
   Бөмбөлгийн хэлбэр ба өнгийн нягт байдлаар бөглөгдсөн хариуг таньж авна.

7. **Хариуг эрэмбэлэх**  
   25 мөр × 2 багана хэлбэрээр хариуг зохион байгуулна.

8. **Хувилбарыг шалгах**  
   A/B/C/D/E хувилбарыг баталгаажуулна.  

9. **Шалгалт хийх**  
   Хэрвээ хувилбар таарч байвал хариуг үнэлнэ.  
   Хувилбар таарахгүй бол уг хариуг алгасна.

10. **Тайлан гаргах**  
    TXT, JSON, болон дүрсээр дэлгэрэнгүй тайлан бэлтгэнэ.

11. **Нийт дүнгийн харьцуулалт**  
    3×4 хэмжээтэй мастер grid-д бүх үр дүнг харьцуулна.

---

## Features

### Ultra-Robust Paper Detection
9 өөр edge detection стратеги ашиглаж тестийн хуудсыг илрүүлнэ:
- **CLAHE enhancement** - Adaptive histogram equalization
- **Multiple Canny edges** - 5 өөр threshold combination
- **Threshold methods** - OTSU, Binary, Adaptive
- **Sobel gradients** - Edge direction detection
- **Scoring system** - Area, rectangularity, angle, center position

```python
# Дөрвөн өнцөг илрүүлэлтийн score тооцоолол
score = (area ** 0.9) * (rectangularity ** 1.4) * 
        (angle_score ** 1.1) * (center_score ** 0.6) * 
        (edge_penalty ** 1.8) * left_edge_bonus
```

### Intelligent Bubble Detection
Circularity болон intensity шинжилгээ:
- **Size filtering**: 80-2500 pixels area
- **Aspect ratio check**: 0.4-2.0 range
- **Circularity score**: `4π × area / perimeter²`
- **Interior intensity**: Center region darkness check
- **Duplicate removal**: Position-based filtering

### Grid Organization Algorithm
Y-threshold ашиглан 25 мөр × 2 багана бүлэглэх:
```python
y_threshold = max(18, avg_bubble_height * 0.75)
```
- Мөр илрүүлэлт - Y координатаар
- Баганы ялгалт - Том зайгаар split
- 5 сонголттой асуулт бүрээс

### Variant Validation System
A, B, C, D, E хувилбарыг шалгаж таньж:
- Top-right region дээр хайна
- Contrast-based detection
- Confidence scoring
- **VARIANT MISMATCH DETECTION** - Буруу хувилбарыг автоматаар илрүүлнэ

### Orientation Detection
3 voting mechanism ашиглан 180° эргэх шаардлагыг шалгана:
- **Content density** - Top vs bottom текст хэмжээ
- **Bubble distribution** - Хаана бөмбөлөг их байна
- **Edge density** - Ирмэгийн тархалт

---

## Технологи & Алгоритм

<div align="center">

| Компонент | Технологи | Хэрэглээ |
|-----------|----------|----------|
| **Paper Detection** | OpenCV Canny, CLAHE, Morphology | 9 стратеги, scoring system |
| **Perspective Transform** | `getPerspectiveTransform` | 4-point warping + padding |
| **Bubble Detection** | Adaptive Threshold, Contours | Circularity + intensity |
| **Grid Organization** | DBSCAN-inspired clustering | Y-threshold grouping |
| **Variant Detection** | ROI extraction, Contour analysis | Top-right region scan |
| **Answer Matching** | NumPy array comparison | Intensity-based selection |
| **Grading Logic** | Python dictionaries | Key-value comparison |
| **Visualization** | Matplotlib + OpenCV drawing | Multi-panel grids |
| **Reports** | JSON + TXT export | Structured data output |

</div>

### Алгоритмын Онцлог

**Paper Detection Scoring:**
```python
score = (area ** 0.9) × (rectangularity ** 1.4) × 
        (angle_score ** 1.1) × (center_score ** 0.6) × 
        (edge_penalty ** 1.8) × left_edge_bonus
```

**Bubble Circularity Check:**
```python
circularity = 4π × area / (perimeter²)
# Шалгуур: circularity > 0.3
```

**Filled Bubble Detection:**
```python
if min_intensity < 180:
    confidence = 0.9
elif min_intensity < 195 and contrast >= 5:
    confidence = 0.85
# ... 6 түвшний threshold
```

---

## Тохиргоо & Параметрүүд

### Bubble Detection Parameters

```python
# Bubble хэмжээний хязгаар
MIN_AREA = 80        # Хамгийн бага талбай (пиксел²)
MAX_AREA = 2500      # Хамгийн их талбай (пиксел²)

# Circularity шалгуур
MIN_CIRCULARITY = 0.3  # Дугуй хэлбэрийн шалгуур

# Aspect ratio
MIN_ASPECT = 0.4      # Өргөн/өндөр хамгийн бага
MAX_ASPECT = 2.0      # Өргөн/өндөр хамгийн их
```

### Grid Organization Parameters

```python
# Y-threshold тооцоолол
y_threshold = max(18, avg_bubble_height * 0.75)

# Мөрийн тоо
TOTAL_ROWS = 25       # 25 асуултын мөр
QUESTIONS_PER_ROW = 2 # Мөр бүрт 2 асуулт (L+R)
TOTAL_QUESTIONS = 50  # Нийт 50 асуулт
CHOICES_PER_Q = 5     # Асуулт бүр 5 сонголт (A-E)
```

### Region Extraction

```python
# Question region
crop_y1 = int(h * 0.08)  # Top: 8%
crop_y2 = int(h * 0.98)  # Bottom: 98%
crop_x1 = int(w * 0.05)  # Left: 5%
crop_x2 = int(w * 0.65)  # Right: 65%

# Variant region
y_start = int(h * 0.06)  # Top: 6%
y_end = int(h * 0.22)    # Bottom: 22%
x_start = int(w * 0.60)  # Left: 60%
x_end = int(w * 0.92)    # Right: 92%
```

---

## Result

### Student Report (TXT формат)

```
================================================================================
✓ GRADING SUCCESSFUL
================================================================================

STUDENT: Student_1
VARIANT: A (VERIFIED ✓)
SCORE: 42/50 (84.0%)
================================================================================

SUMMARY:
  ✓ Correct:   42 (84.0%)
  ✗ Incorrect:  5 (10.0%)
  ○ Blank:      3 (6.0%)

================================================================================

DETAILED BREAKDOWN:
────────────────────────────────────────────────────────────────────────────────
Q#    Student    Correct    Status         
────────────────────────────────────────────────────────────────────────────────
1     A          A          ✓ correct
2     B          B          ✓ correct
3     C          D          ✗ incorrect
4     -          A          ○ blank
...
```
![](images/result.png)

### Overall Summary Statistics

```
STATISTICS:
  Average score: 38.2/50
  Average percentage: 76.4%
  Highest score: 45/50 (90.0%)
  Lowest score: 32/50 (64.0%)
  Median score: 39.0/50
  Standard deviation: 8.3%
```