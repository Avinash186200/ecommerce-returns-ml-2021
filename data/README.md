# Data

The data is not included in this repo. The notebook expects two CSV files:

- `TrainingData_V1.csv` - labelled orders
- a test CSV with the same columns except `return` - orders to score

Columns:

| Column | Meaning |
|---|---|
| `order_item_id` | id of the order line |
| `order_date` | date of the order (dd-mm-yyyy in train, mm/dd/yyyy in test) |
| `delivery_date` | date of delivery, may be missing |
| `item_id` | product id |
| `item_size` | size label |
| `item_color` | colour label |
| `brand_id` | brand id |
| `item_price` | price paid |
| `user_id` | customer id |
| `user_title` | customer title |
| `user_dob` | customer date of birth |
| `user_state` | customer region |
| `user_reg_date` | account creation date |
| `return` | target: 1 = returned, 0 = kept (train only) |
