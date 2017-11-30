# Convert Any Digital Text Into Natural Sounding Speech
> Chris Burns  
> Tomasz Stachlewski  

## Notes

### Amazon Polly
> [Blog Post](https://aws.amazon.com/blogs/ai/create-audiobooks-with-amazon-polly-and-aws-batch/)

* Overcoming 1500 characters limit and creating TTS application using Polly and Batch services
    * First approach
        * Text > S3 > Lambda > Polly
    * Solution
        * Text > S3 > Lambda > Batch > Polly > SNS

### Amazon Comprehend

* Sentiment analysis (SSML)
    * Architecture
        * Text > S3 > Lambda > Comprehend > Text + SNT > S3 > Lambda > Polly
            * Use Lambda to send text to Comprehend
            * Receive sentiment analysis from Comprehend
            * Use Lambda to inject SSML into text
            * Send text with SSML to Polly
