# Video Game Sales Data Analysis Using Python

## Data Analysis with MySQL and Pandas

### Overview

This project demonstrates how to interact with a MySQL database, extract data, and perform data analysis using Python's Pandas library. The project uses a MySQL connection to retrieve video game sales data and performs several operations such as querying, aggregating, and transforming the data to gain insights.

## Requirements

- Python 3.x
- Pandas
- SQLAlchemy
- PyMySQL
- A running MySQL instance

## Setup

### 1. Install Dependencies

You can install the necessary Python libraries by running the following command:

```bash
pip install pymysql pandas sqlalchemy
```

### 2. MySQL Setup

Make sure you have a MySQL server running and have access to the database `data1202` with the relevant credentials. If not, you can create a new database and import the required tables.

### 3. Database Configuration

Modify the database connection string in the following line of the code with your MySQL username, password, and host:

```python
import os
from sqlalchemy import create_engine

user = os.getenv('DB_USER')
password = os.getenv('DB_PASSWORD')
host = os.getenv('DB_HOST', 'localhost')
database = os.getenv('DB_NAME', 'DB_NAME')

engine = create_engine(f'mysql+pymysql://{user}:{password}@{host}/{database}')
conn = engine.connect()

```
Set Environment Variables before running the script:

```python
export DB_USER="your_username"
export DB_PASSWORD="your_password"
export DB_HOST="your_host"
export DB_NAME="DB_NAME"
```

## Code Explanation

### Step 1: Establishing a Connection

The project connects to a MySQL database using SQLAlchemy and PyMySQL. We use the following code to create a connection:

```python
import os
from sqlalchemy import create_engine

user = os.getenv('DB_USER')
password = os.getenv('DB_PASSWORD')
host = os.getenv('DB_HOST', 'localhost')
database = os.getenv('DB_NAME', 'DB_NAME')

engine = create_engine(f'mysql+pymysql://{user}:{password}@{host}/{database}')
conn = engine.connect()

```

### Step 2: Querying the Database

The project demonstrates querying the database using both simple and complex SQL queries.

#### Simple Query: Fetches all records from the `vgsales1 (1)` table and displays the first 10 rows.

```python
df = pd.read_sql_query("SELECT * FROM data1202.`vgsales1 (1)`", conn)
df.head()
```

#### Aggregated Query: Sums up the sales by region and calculates the percentage share of North American sales.

```python
complex_df = pd.read_sql_query('''
    SELECT
        ROUND(SUM(NA_Sales)) as 'NA_Sales',
        ROUND(SUM(EU_Sales)) as 'EU_Sales',
        ROUND(SUM(JP_Sales)) as 'JP_Sales',
        ROUND(SUM(Global_Sales)) AS 'Global_Sales',
        ROUND((SUM(NA_Sales)/SUM(Global_Sales)) * 100) as 'NA_GlobalShare'
    FROM data1202.`vgsales1 (1)`
    WHERE Genre = 'Action' AND Year >= 2005''', conn)
```

#### Categorizing Data: Uses a CASE statement to categorize the data into pre-2005 and post-2005 sales periods.

```python
query1 = pd.read_sql_query(
    """
    SELECT 
        CASE 
            WHEN Year < 2005 THEN 'pre-2005'
            ELSE 'post-2005'
        END AS Sales_Period,
        AVG(Global_Sales) AS Avg_Global_Sales
    FROM `vgsales1 (1)`
    GROUP BY Sales_Period
    """, conn)
```

### Step 3: Data Analysis

Once the data is fetched into a DataFrame, various Pandas functions are used to explore and analyze the dataset:

- **Data Inspection:** Checking the shape, size, columns, and data types of the dataset.
- **Query Execution:** Running SQL queries directly into a Pandas DataFrame for analysis.

```python
df.shape  # Displays the shape of the DataFrame (number of rows and columns)
df.size   # Displays the number of elements in the DataFrame
df.columns  # Displays the column names of the DataFrame
df.dtypes  # Displays the data types of the columns
```

### Step 4: Output

After running the queries and transforming the data, the results are stored in DataFrames and can be further analyzed or saved for reporting.

#### Example Output

| NA_Sales | EU_Sales | JP_Sales | Global_Sales | NA_GlobalShare |
|----------|---------|---------|--------------|---------------|
| 579.0    | 378.0   | 107.0   | 1208.0       | 48.0          |

## Contributions

Feel free to fork the repository and make contributions. If you have suggestions or improvements, please submit a pull request.
The dataset is also avaialble in this repository.

## Acknowledgement

Special thanks to the open-source community and all contributors who helped make this project possible.

## Author

Caniya Sabu

## License

This project is open-source and available under the MIT License.

