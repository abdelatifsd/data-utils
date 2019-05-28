# Data-utils
In an effort to simplify the AI's team work, this repo will be used to seamlessly acquire the data from the databases.

For now, this module is for a specific use case where we imports a compressed csv from an s3 bucket into a dataframe from pandas or csv format.

Can also be used alongside django projects to import data inside of a specified model's table.

## Usage Example
```py
import os

from datetime import datetime, timedelta
import s3.utils as du # du for data_utils

YESTERDAY = (datetime.today() - timedelta(1)).strftime("%Y-%m-%d")

df = du.import_s3_csv_to_df(
    _aws_key=os.getenv("AWS_ACCESS_KEY_ID"),
    _secret_key=os.getenv("AWS_SECRET_ACCESS_KEY"),
    _bucket=os.getenv("BUCKET"),
    _region='eu-west-1',
    key=f'Dkt_canada/data/sport_popularity/city_sport_{YESTERDAY}_000.gz'
)


du.convert_df_to_csv(df, filepath='./sports.csv')
```