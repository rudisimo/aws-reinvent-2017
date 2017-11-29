# Serverless Encoding at Scale with Content Moderation via Deep Learning-Based Video Analysis
> Paul Roberts  

# Notes

* Linear-television business model evolution
* Transcoding at scale with AWS
    * Traditional encoding approach
        * Media Content > Server Farm > Clients
    * Transcoding at scale
        * Rekognition
            * Add deep learning-based image analysis to encoding
            * Why use it?
                * Object and scene detection
                    * DetectLabels: analyze objects in images and label them
                * Facial analysis
                    * DetectFaces: analyze facial characteristics in multiple dimension
                * Face comparison
                * Facial recognition
                    * SearchFacesByImage: find similar faces in large collections of images
                    * RekognizeCelebrities: recognize thousands of famous celebrities
                * Text detection
* Benefits
    * On-demand resources
    * Scale-out compute
    * No server management
    * No idle capacity
    * Highly available by design
    * Simple integration with AWS AI services
* Content detection via deep-learning video analysis
    * Metadata extraction from video frames
        * Detect facial features
        * Detect emotion
        * Detect gender
        * Detect eye position and state
        * Detect adult content
* What does a typical architecture look like
    * S3 > Lambda > MediaConvert > Lambda > Rekognition / DynamoDB 
