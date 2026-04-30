# 🧮 DAX Measures & Calculations

## 📊 Total Bill Amount
Calculates total billing amount excluding discounts.

```DAX
Total_Bill_Amt = 
CALCULATE(
    SUM(bills[Value]),
    bills[Charge_Type] <> "Discount"
)


## 📊 Total quantity of medicines sold to patients


Total_Med_Sale_QTY = 
SUM(medicine_patient[qty])


## ⭐ Patient Satisfaction Rating (Star Format)

This measure converts numeric satisfaction ratings into a 5-star visual representation using Unicode characters.

- Normalizes rating between 0–5  
- Converts value into stars (★ ☆)  
- Enhances dashboard readability  

```DAX
Sum of satisfaction_rating star rating = 

VAR _MAX_NUMBER_OF_STARS = 5
VAR _MIN_RATED_VALUE = 0
VAR _MAX_RATED_VALUE = 5

VAR _BASE_VALUE = SUM('patient_info'[satisfaction_rating])

VAR _NORMALIZED_BASE_VALUE =
    MIN(
        MAX(
            DIVIDE(
                _BASE_VALUE - _MIN_RATED_VALUE,
                _MAX_RATED_VALUE - _MIN_RATED_VALUE
            ),
            0
        ),
        1
    )

VAR _STAR_RATING =
    ROUND(_NORMALIZED_BASE_VALUE * _MAX_NUMBER_OF_STARS, 0)

RETURN
IF(
    NOT ISBLANK(_BASE_VALUE),
    REPT(UNICHAR(9733), _STAR_RATING) &
    REPT(UNICHAR(9734), _MAX_NUMBER_OF_STARS - _STAR_RATING)
)


## Custom date table for time-based analysis

```DAX
Calendar = 
ADDCOLUMNS(
    CALENDARAUTO(),
    "Month", FORMAT([Date], "MMM"),
    "Month-Index", MONTH([Date]),
    "Month-Yr", FORMAT([Date], "mmm-yy"),
    "Day", FORMAT([Date], "ddd"),
    "Formatted_Date", FORMAT([Date], "dd-mmm-yy")
)
