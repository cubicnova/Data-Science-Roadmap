# Data Dictionary

All datasets in this folder are for educational practice. Except for the classic Iris dataset (if generated from scikit-learn), the records are synthetic.

## students.csv
- `student_id`: synthetic student identifier.
- `student_name`: generated display name.
- `age`: age in years.
- `class`: class group.
- `attendance_percentage`: attendance from 0 to 100.
- `study_hours_per_day`: illustrative daily study hours.
- `exam_score`: illustrative score from 0 to 100.

## sales.csv
- `transaction_id`: synthetic transaction identifier.
- `date`: transaction date (YYYY-MM-DD).
- `customer_id`: synthetic customer identifier.
- `product`: product name.
- `quantity`: units purchased.
- `unit_price_inr`: price per unit in INR.
- `discount_rate`: decimal discount (e.g. 0.10 means 10%).
- `revenue_inr`: quantity × unit price after discount.
- `sales_channel`: illustrative sales channel.

## employees.csv
- `employee_id`: synthetic employee identifier.
- `employee_name`: generated label.
- `department`: department name.
- `job_level`: illustrative job level.
- `age`: age in years.
- `years_experience`: years of experience.
- `annual_salary_inr`: illustrative annual salary in INR.
- `city`: illustrative city label.
- `employment_type`: employment arrangement.

## house_prices.csv
- `house_id`: synthetic property identifier.
- `area_sqft`: property area in square feet.
- `bedrooms`: number of bedrooms.
- `bathrooms`: number of bathrooms.
- `property_age_years`: age of property in years.
- `location`: illustrative location category.
- `price_inr`: synthetic target price in INR.

## iris.csv
- `sepal_length_cm`: sepal length in centimeters.
- `sepal_width_cm`: sepal width in centimeters.
- `petal_length_cm`: petal length in centimeters.
- `petal_width_cm`: petal width in centimeters.
- `species`: target flower species.

## customer_churn.csv
- `customer_id`: synthetic customer identifier.
- `tenure_months`: customer tenure in months.
- `contract_type`: contract duration category.
- `monthly_charges_inr`: illustrative monthly charges in INR.
- `service_plan`: subscription plan category.
- `support_tickets_last_year`: number of support tickets.
- `payment_method`: payment method category.
- `churn`: illustrative binary target (`Yes` or `No`).

## weather.csv
- `date`: observation date (YYYY-MM-DD).
- `temperature_c`: illustrative temperature in Celsius.
- `humidity_percentage`: relative humidity percentage.
- `rainfall_mm`: rainfall in millimeters.
- `wind_speed_kmh`: wind speed in kilometers per hour.
- `condition`: illustrative weather category.

## products.json
Each record contains:
- `product_id`: synthetic product identifier.
- `name`: product name.
- `category`: product category.
- `price_inr`: price in INR.
- `in_stock`: illustrative stock availability.
- `tags`: list of descriptive tags.
