# Data-utils
In an effort to simplify the AI's team work, this repo will be used to seamlessly acquire the data from the databases.

For now, this module is for a specific use case where we imports a compressed csv from an s3 bucket into a dataframe from pandas or csv format.

Can also be used alongside django projects to import data inside of a specified model's table.

## Installation
The installation requires the [setuptools](https://github.com/pypa/setuptools) package to be installed on your computer

- You will need to clone this project `git clone git@github.com:dktunited/data-utils.git`
- then, you will need to run `python setup.py install`

## Public functions
##### `data_utils.df`
- `import_s3_csv_to_df`: takes in an s3 client and a key and returns a dataframe
- `convert_df_to_csv`: saves a dataframe to a specified csv filepath
- `convert_df_to_django_model`: if you use this in a django project, you can directly save a df to a model's table
- `convert_df_to_s3_compressed_csv`: takes in an s3 client and a df and saves it in s3 as a compressed csv
##### `data_utils.geoloc`
- `map_df_to_kd_tree`: map the dataframe to a kdtree, for faster 3d search
- `calculate_haversine_distance`: calculates the great circle distance between two points on Earth

## Usage Example
```py
import os

from dotenv import load_dotenv
import boto3

import data_utils.df as du  # du for data_utils

load_dotenv()

# Connect to the s3 bucket and extract the compressed csv at the key
session = boto3.session.Session(region_name='eu-west-1')
s3client = session.client(
    's3',
    aws_access_key_id=os.getenv("AWS_ACCESS_KEY_ID"),
    aws_secret_access_key=os.getenv("AWS_SECRET_ACCESS_KEY"),
)

df = du.import_s3_csv_to_df(
    s3client=s3client,
    bucket=os.getenv("BUCKET"),
    key=f'Dkt_canada/shawn_test/test_000.gz'
)

du.convert_df_to_csv(df, filepath='./test.csv')
```