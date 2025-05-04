# Real‑Time Stock Pipeline on AWS

A hands‑on data engineering project to ingest, process, store, and visualize real‑time stock data for the MAG7 tickers (with capacity to scale). Built on AWS and documented for reproducibility.

## Features

- Ingests raw tick data for a configurable list of symbols (MAG7 by default)  
- Aggregates into 1‑second OHLC bars  
- Computes real‑time VWAP and can be extended with additional indicators  
- Stores raw and transformed series in Amazon Timestream (7‑day raw retention)  
- Replays missed or late data on demand  
- Publishes to a live QuickSight dashboard for monitoring and alerts  
- Infrastructure as Code via AWS CDK / CloudFormation  

## Technologies Used

- **AWS Services**  
  - Kinesis Data Streams (ingest)  
  - AWS Lambda (stream processing & transformations)  
  - Amazon Timestream (time‑series storage)  
  - Amazon QuickSight (dashboarding)  
  - CloudWatch (metrics & logs)  
  - AWS CDK or CloudFormation (IaC)  

- **Local Tools & Libraries**  
  - Windows 11 + Visual Studio Code  
  - Python 3.9+ (`boto3`, `pandas`)  
  - Git & GitHub CLI  
  - AWS CLI  

## Project Structure

    realtime-stock-pipeline-aws/
    ├── infra/                     # AWS CDK stacks or CloudFormation templates
    │   ├── cdk.json
    │   └── stacks/
    │       └── pipeline-stack.ts
    ├── src/
    │   ├── producer.py            # Fetches 1 sec bars from IEX and sends to Kinesis
    │   ├── processor.py           # Lambda handler: computes VWAP and writes to Timestream
    │   └── utils.py               # Shared config loaders and helpers
    ├── config/
    │   └── example.env            # AWS_PROFILE, AWS_REGION, IEX_TOKEN
    ├── README.md                  # This documentation
    ├── requirements.txt           # Python dependencies
    └── .gitignore                 # Excludes `.env`, virtual environments, etc.

## How to Use

1. **Clone the repo**  
       git clone git@github.com:<your‑username>/realtime-stock-pipeline-aws.git  
       cd realtime-stock-pipeline-aws

2. **Set up Python environment**  
       python -m venv .venv  
       .\.venv\Scripts\Activate.ps1  
       pip install --upgrade pip  
       pip install -r requirements.txt

3. **Configure AWS CLI & credentials**  
       aws configure  
   Ensure your IAM user has Kinesis, Timestream, QuickSight, Lambda, CloudWatch, and CDK/CloudFormation permissions.

4. **Populate environment variables**  
   Copy `config/example.env` → `.env` and fill in:  
       AWS_PROFILE=default  
       AWS_REGION=us-east-1  
       IEX_TOKEN=pk_…  
   (`.env` is in `.gitignore`)

5. **Deploy infrastructure**  
       cd infra  
       cdk deploy   # or: aws cloudformation deploy --template-file pipeline.yaml …

6. **Run the producer locally**  
       cd ..  
       python src/producer.py

7. **Verify data flow**  
   - Check Kinesis stream metrics in CloudWatch  
   - Inspect raw and VWAP tables in Timestream  
   - Open QuickSight and build/view the dashboard

## Notes

- Store sensitive values only in `.env` (excluded via `.gitignore`).  
- You can extend `processor.py` with additional transforms (EMA, Bollinger Bands, etc.).  
- Replay logic for missing shards can be enabled via Kinesis Data Streams APIs.

## Author

Ryan Winters

