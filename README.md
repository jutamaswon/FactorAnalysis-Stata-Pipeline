# การวิเคราะห์ข้อมูลทางสถิติและการวิจัยขั้นสูงด้วย Python (Research Data Analysis Pipeline 2026)

ระบบประมวลผลและวิเคราะห์ข้อมูลงานวิจัยเชิงปริมาณ (Quantitative Research Data Analysis) แบบครบวงจร พัฒนาขึ้นด้วยภาษา Python เพื่อทำการวิเคราะห์องค์ประกอบ (Factor Analysis), สถิติพรรณนา, สถิติทดสอบเปรียบเทียบกลุ่ม (Bivariate Analysis: Independent Samples t-test & ANOVA / F-test พร้อม Smart Grouping), การวิเคราะห์การถดถอยเชิงเส้น (Multiple Linear Regression) ตลอดจนการแปลงและส่งออกชุดข้อมูลไปยังโปรแกรม **Stata** (รองรับทั้ง Stata 12 Format 114 และ Stata 14+ Format 118 UTF-8)

---

## 📌 สรุปคุณสมบัติเด่น (Key Features)

1. **การจำแนกตัวแปรและโครงสร้างปัจจัยอัตโนมัติ (Automated Variable & Factor Detection):**
   - คัดแยกตัวแปรประชากรศาสตร์ ($a_1 - a_9$) และตัวแปรมาตรวัด Likert 5 ระดับ ($b_1 - b_{28}$) อัตโนมัติ
   - ดึงชื่อและนิยามข้อคำถามจาก Codebook (`Sheet1`) เพื่อจัดกลุ่มปัจจัยทฤษฎี 7 ปัจจัยโดยไม่ต้อง Hardcode
2. **การวิเคราะห์องค์ประกอบและตรวจสอบคุณภาพเครื่องมือ (Factor Analysis & Reliability Test):**
   - คำนวณค่าน้ำหนักองค์ประกอบ (Factor Loadings)
   - คำนวณความแปรปรวนสะสม (% Total Variance Explained)
   - คำนวณค่าความเที่ยงตรงภายใน (Cronbach's Alpha)
   - คำนวณค่าความแปรปรวนเฉลี่ยที่สกัดได้ (AVE: Average Variance Extracted)
   - คำนวณค่าความเชื่อถือได้เชิงประกอบ (CR: Composite Reliability)
3. **การคำนวณคะแนนองค์ประกอบที่ตรงตามมาตรฐาน Stata (Accurate Factor Scores):**
   - แก้ไขปัญหาทิศทางเวกเตอร์ (Eigenvector Sign Alignment) ทำให้ค่าคะแนน Factor Scores ($x_1 - x_7$) สอดคล้องกับค่าเฉลี่ยตัวแปรจริง และตรงกับผลลัพธ์ของ Stata 100% ($r = 1.000000$, ความคลาดเคลื่อน $< 10^{-7}$)
4. **การทดสอบความแตกต่างระหว่างกลุ่มด้วย Smart Grouping (Bivariate t-test & F-test):**
   - มีระบบรวมกลุ่มตัวแปรย่อยอัตโนมัติกรณีกลุ่มตัวอย่างย่อยน้อยกว่า 30 ราย ($n < 30$) ตามลำดับหมวดหมู่ เพื่อให้สอดคล้องกับทฤษฎีขีดจำกัดส่วนกลาง (Central Limit Theorem)
   - รองรับการรายงานผลทั้งแบบ **Smart Grouping** และ **Standard (Un-grouped)**
5. **การส่งออกข้อมูลเชื่อมต่อกับ Stata (Full Stata Integration):**
   - บันทึกไฟล์ข้อมูล `.dta` ที่มีตัวแปรครบถ้วน ทั้งตัวแปรดิบ, ค่าเฉลี่ยรายด้าน, และ Factor Scores ($x$)
   - รองรับทั้ง Stata 14+ (`version=118` เข้ารหัส UTF-8 ภาษาไทยสมบูรณ์) และ Stata 10–12 (`version=114`)

---

## 📂 โครงสร้างไฟล์ใน Repository (Project Structure)

```text
├── README.md                      # เอกสารอธิบายรายละเอียดโครงการและการใช้งาน (ภาษาไทย)
├── data.xlsx                      # ไฟล์ข้อมูลแบบสอบถามดิบ (403 ตัวอย่าง, 39 คอลัมน์ พร้อม Sheet1 Codebook)
└── python/
    ├── factoranalysis.ipynb       # Jupyter Notebook หลักที่บรรจุโค้ดทุกขั้นตอน (Step 0 - Step 9)
    └── data.xlsx                  # ชุดข้อมูลแบบสอบถาม (สำหรับรันภายในโฟลเดอร์ python/)
```

---

## 🔬 โครงสร้างตัวแปรในงานวิจัย

ชุดข้อมูลการวิจัยประกอบด้วยกลุ่มตัวอย่าง $N = 403$ ราย แบ่งออกเป็น 2 ส่วนหลัก:

### 1. ข้อมูลทั่วไปและประชากรศาสตร์ (Demographic Variables: $a_1 - a_9$)
- **a1**: เพศ (Gender)
- **a2**: อายุ (Age)
- **a3**: ระดับการศึกษา (Education)
- **a4**: อาชีพ (Occupation)
- **a5**: รายได้เฉลี่ยต่อเดือน (Monthly Income)
- **a6 - a9**: พฤติกรรมและข้อมูลทั่วไปของผู้บริโภค

### 2. มาตรวัดความคิดเห็นและพฤติกรรม (Likert Scale 1–5: $b_1 - b_{28}$)
ถูกจัดเป็น 7 ปัจจัยเชิงทฤษฎี (Theoretical Factor Groups):
1. **Factor 1: ด้านการรับรู้แบรนด์ร่วม (Awareness of co-branded brands)** — 5 ข้อคำถาม ($b_1 - b_5$)
2. **Factor 2: ด้านความเข้ากันได้ของแบรนด์ร่วม (Brand matching of co-branded brands)** — 4 ข้อคำถาม ($b_6 - b_9$)
3. **Factor 3: ด้านความภักดีต่อแบรนด์ร่วม (Loyalty to co-branded brands)** — 4 ข้อคำถาม ($b_{10} - b_{13}$)
4. **Factor 4: ด้านความหายากของสินค้าแบรนด์ร่วม (Scarcity of co-branded products)** — 3 ข้อคำถาม ($b_{14} - b_{16}$)
5. **Factor 5: คุณค่าทางสังคม (Social Value)** — 4 ข้อคำถาม ($b_{17} - b_{20}$)
6. **Factor 6: ด้านคุณค่าทางอารมณ์ (Emotional value)** — 4 ข้อคำถาม ($b_{21} - b_{24}$)
7. **Factor 7: ด้านความเต็มใจซื้อ (Willingness to buy)** — 4 ข้อคำถาม ($b_{25} - b_{28}$)

---

## 🚀 ขั้นตอนการประมวลผล (Step-by-Step Workflow)

ไฟล์ [python/factoranalysis.ipynb](file:///python/factoranalysis.ipynb) แบ่งการทำงานออกเป็นขั้นตอนที่ชัดเจน ดังนี้:

| ขั้นตอน (Step) | วัตถุประสงค์และการทำงาน | ไฟล์ผลลัพธ์ที่ได้ (Output) |
| :--- | :--- | :--- |
| **Step 0** | รองรับการเชื่อมต่อกับ Google Drive ในกรณีที่รันผ่าน Google Colab | - |
| **Step 1** | ตรวจสอบและติดตั้ง Library ที่จำเป็น (`pandas`, `factor_analyzer`, `pingouin` ฯลฯ) | - |
| **Step 2 + 3** | ทำความสะอาดข้อมูล (Data Cleansing) คัดกรองตัวแปรที่ไม่เกี่ยวข้อง และวิเคราะห์ Factor Analysis | `Table2FactorAnalysis.xlsx` |
| **Step 4** | คำนวณคะแนนองค์ประกอบ (Factor Scores $x_1 - x_7$) พร้อมจัดทิศทาง Eigenvector Sign | บันทึกลงในหน่วยความจำ DataFrame |
| **Step 5** | สร้างตารางไขว้สถิติพรรณนา (Crosstabs) และสถิติเชิงบรรยายของตัวแปร Likert (Mean, SD) | `Table1_Crosstabs.xlsx`, `Table3_Frequency_Likert.xlsx` |
| **Step 6 + 7** | ทดสอบความแตกต่างของ Factor Scores จำแนกตามข้อมูลทั่วไป (t-test / ANOVA) ทั้งแบบปกติและ Smart Grouping | `Table4_ttest_Ftest_Results.xlsx`, `Table4_ttest_Ftest_SmartGrouping.xlsx` |
| **Step 8** | การวิเคราะห์การถดถอยเชิงพหุ (Multiple Linear Regression) เพื่อทดสอบสมมติฐาน | `Table5_Regression_Results.xlsx` |
| **Step 9** | ส่งออกชุดข้อมูลพร้อมผลการวิเคราะห์ในรูปแบบไฟล์ Stata ทั้งเวอร์ชันใหม่และเก่า | `data2.dta`, `data2_stata12.dta` |

---

## 🛠️ ความต้องการของระบบและการติดตั้ง (Prerequisites & Installation)

### 1. สภาพแวดล้อมระบบ (Environment)
- Python เวอร์ชั่น 3.10, 3.11 หรือ 3.12
- สามารถรันได้ทั้งบน Local Machine (Jupyter Lab / VS Code) และ Google Colab

### 2. การติดตั้งแพ็กเกจ (Install Dependencies)
สามารถติดตั้งไลบรารีที่จำเป็นผ่านคำสั่ง Terminal / PowerShell ดังนี้:

```bash
pip install pandas numpy scipy scikit-learn factor_analyzer pingouin openpyxl statsmodels tabulate
```

หรือรันผ่านเซลล์แรกใน Jupyter Notebook:
```python
%pip install pandas numpy factor_analyzer pingouin openpyxl scipy statsmodels
```

---

## 💻 วิธีการเปิดใช้งานและการรัน (How to Run)

1. **Clone หรือดาวน์โหลด Repository:**
   ```bash
   git clone https://github.com/jutamaswon/Python2026.git
   cd Python2026
   ```
2. **เปิดไฟล์ Jupyter Notebook:**
   - เปิดโฟลเดอร์โปรเจกต์ด้วย VS Code หรือ Jupyter Lab
   - เปิดไฟล์ `python/factoranalysis.ipynb`
3. **รันเซลล์ตามลำดับ:**
   - สามารถกด **Run All** เพื่อประมวลผลตั้งแต่ต้นจนจบ
   - ไฟล์ตารางสรุปผลลัพธ์ Excel (`Table1` ถึง `Table5`) และไฟล์ Stata (`data2.dta`) จะถูกสร้างขึ้นในโฟลเดอร์ทำงานโดยอัตโนมัติ

---

## 📊 ตารางเปรียบเทียบและการตรวจสอบความถูกต้อง (Verification)

ผลลัพธ์การคำนวณจาก Python ในโปรเจกต์นี้ได้รับการตรวจสอบความสอดคล้องกับ Do-files และผลลัพธ์ของ **Stata** (`1.sample group.do`, `3.bivariate.do`, `data2.dta`):
- **Factor Loadings & Variance:** ตรงกันตามมาตรฐาน Principal Component / Factor Analysis
- **Factor Scores ($x_1 - x_7$):** สัมประสิทธิ์สหสัมพันธ์ $r = 1.000000$ กับ Stata `data2.dta`
- **Mean Factor by Category:** ตัวอย่างเช่น $a_1$ เพศชาย = 0.6415, เพศหญิง = 0.6693 ตรงกับ Stata `Table4.xls` (เซลล์ D60, D61)
- **Sample Size Integrity:** ตรวจสอบขนาดกลุ่มตัวอย่างคงที่ $N = 403$ ตลอดทั้งกระบวนการ

---

## 👤 จัดทำโดย (Author)
- **หลักสูตรบริหารธุรกิจดุษฎีบัณฑิต / มหาบัณฑิต (RCIM / MBA)**
- มหาวิทยาลัยเทคโนโลยีราชมงคลรัตนโกสินทร์ (RMUTR)
