# Media Intelligence for the Cloud with Amazon AI
> Konstantin Wilms  
> Dean Perrine  

## Notes

### National Geographic

* Problem
    * 129 years of content
    * Petabytes of images and metadata
* AWS Migration:
    * Editing and publishing
    * Video streaming
    * Web applications
* Deep Learning-Based Image Analysis
    * Search, Verify, and Organize
* Image Archive Challenge
    * Niche image categories
        * Great candidate for Mechanical Turk
        * Images with not enough resolution
        * Images with text
    * Image optimization
        * Image enhancement & stabilization with increase label accuracy
        * Resizing (Beamr JPGmini, PNGQuant, Mozilla Mozjpeg)
        * Unsharp mask
        * Deblur
        * Refocus
    * Low & ultra-high resolutions
    * Artifacts & noise removal
    * Black & white footage
    * Historical context
    * High accuracy required
* Media Intelligence Pipeline
    * Ingest
        * Identify object, scenes, concepts, and provide confidence scores
        * Self-service, multi-tenant for internal teams
        * Stored metadata available via an API
        * Automatic image resizing
        * UUID generation for global ingests
    * Store
    * Analyze
        * Metadata enrichment through deep learning
        * Dog or muffin?
    * Deliver
    * Components:
        * S3 (Content Archive)
        * Step Functions (Image Processing)
        * Rekognition (Label Detection)
        * ElasticSearch (Realtime Search)
        * DynamoDB (Asset Metadata)
* AWS Service Integration
    * Processing: Elemental MediaConvert & MediaLive
    * Storage: S3 & EFS
    * Compute: Batch, EC2, ECS
* Amazon AI Stack
    * Services: Vision, Speech, Language
    * Platforms: Amazon ML, Spark & EMR, Kinesis, Batch
    * Infrastructure:
        * P2 & P3: Distributed Training & Inference
        * G3: Multi-User Modeling
        * F1: High Speed Inference
        * X1: Specialized AI/ML/DL
* Next Steps
    * Video capability
    * Metadata transformer for varying output requirements
    * Rekognition result differential tracking
    * Integration with existing Web & Mobile applications
