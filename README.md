# Watson Visual Recognition - usage samples via command line (CURL)
 
This document cover different use cases for using Watson Visual Recognition.
Several pictures are used in the documentation, those pictures won't be available in the repository for possible copyrights.
A simple and quick way to collect many images on the same subject is to use 'image' tab in Google. There are several browser pluging to bulk download images from a web site.

Prerequites to run through the scenario is having an instance of Visual Recognition. The service is available as service in IBM Bluemix. It has a free tier that allows everybody to experiment with the service.
Service home page with documentation and online demo) is available at this URL:  
https://www.ibm.com/watson/services/visual-recognition/

In the same page is possible to find a link to the APIs. We will use APIs extensively.  
https://www.ibm.com/watson/developercloud/visual-recognition/api/v3/  
https://watson-api-explorer.mybluemix.net/apis/visual-recognition-v3?cm_mc_uid=11860889192715021454544&cm_mc_sid_50200000=1502274040&cm_mc_sid_52640000=


Once the service is created in Bluemix it's fundamental to get service credential (api_key)

  ![Service credentials](README_images/visual-recognition_credential.jpg)
  
Working from bash command line is handy to create variables and use them from other commands. 
From service credential copy the api_key value and export into api_key variable.

``` sh
$ export api_key="39ce25c61452cadfXXXXXXXXXXXXXXXXXXX"
$ echo ${api_key}
39ce25c61452cadfXXXXXXXXXXXXXXXXXXX 
```

Let's start now with the usage scenarios


## Classify an image using 'default' classifier.

In this scenario we are going to use the default classifier and passing a random image from internet.
For convenience we will store the URL into a variable as we did with the api_key.

  ![Car](README_images/152m-laterale-2560.jpg)

``` sh
$ export image_url=http://static.812superfast.ferrari.com/includes/20170309/img/50.pages.01.homepage/03.Exterior-design/01/152m-laterale-2560.jpg
```  
and then we will invoke the API  
**Note:**  use double quote in CURL

``` sh
$ curl -X GET "https://watson-api-explorer.mybluemix.net/visual-recognition/api/v3/classify?url=${image_url}&api_key=${api_key}&classifier_ids=default&version=2016-05-20"
Request URL
```

The result gives us already a lot of information about the picture:

```
{
    "custom_classes": 0,
    "images": [
        {
            "classifiers": [
                {
                    "classes": [
                        {
                            "class": "sports car",
                            "score": 0.768,
                            "type_hierarchy": "/vehicle/wheeled vehicle/car/sports car"
                        },
                        {
                            "class": "car",
                            "score": 0.924
                        },
                        {
                            "class": "wheeled vehicle",
                            "score": 0.949
                        },
                        {
                            "class": "vehicle",
                            "score": 0.949
                        },
                        {
                            "class": "sedan",
                            "score": 0.534,
                            "type_hierarchy": "/vehicle/wheeled vehicle/car/sedan"
                        },
                        {
                            "class": "motor vehicle",
                            "score": 0.533,
                            "type_hierarchy": "/vehicle/wheeled vehicle/motor vehicle"
                        },
                        {
                            "class": "body (of vehicle)",
                            "score": 0.527
                        },
                        {
                            "class": "roadster",
                            "score": 0.524,
                            "type_hierarchy": "/vehicle/wheeled vehicle/car/roadster"
                        },
                        {
                            "class": "coupe car",
                            "score": 0.5,
                            "type_hierarchy": "/vehicle/wheeled vehicle/car/coupe car"
                        },
                        {
                            "class": "maroon color",
                            "score": 0.752
                        },
                        {
                            "class": "claret red color",
                            "score": 0.584
                        }
                    ],
                    "classifier_id": "default",
                    "name": "default"
                }
            ],
            "resolved_url": "http://static.812superfast.ferrari.com/includes/20170309/img/50.pages.01.homepage/03.Exterior-design/01/152m-laterale-2560.jpg",
            "source_url": "http://static.812superfast.ferrari.com/includes/20170309/img/50.pages.01.homepage/03.Exterior-design/01/152m-laterale-2560.jpg"
        }
    ],
    "images_processed": 1
}
```
  
The same result can be obtained uploading an image instead of passing an URL. Below the CURL syntax, the image is stored in the same folder where the CURL command is executed.

``` sh
  curl -X POST -F "images_file=@motorcycle.jpg" "https://watson-api-explorer.mybluemix.net/visual-recognition/api/v3/classify?api_key=${api_key}&classifier_ids=default&version=2016-05-20"
```

As before we got a response from the default classifier with information about the object in the image:

```
{
    "custom_classes": 0,
    "images": [
        {
            "classifiers": [
                {
                    "classes": [
                        {
                            "class": "motorcycle",
                            "score": 0.979,
                            "type_hierarchy": "/vehicle/wheeled vehicle/motorcycle"
                        },
                        {
                            "class": "wheeled vehicle",
                            "score": 0.979
                        },
                        {
                            "class": "vehicle",
                            "score": 0.979
                        },
                        {
                            "class": "maroon color",
                            "score": 0.962
                        }
                    ],
                    "classifier_id": "default",
                    "name": "default"
                }
            ],
            "image": "motorcycle.jpg"
        }
    ],
    "images_processed": 1
}
```

## Recognize a face using 'default' classifier.

Visual recognition offers a specific API to detect faces. The API return the caracteristic of the person in the picture and in many cases can detect whether it's a celebrity.

As before just find a picture from internet and save the URL in a variable:

  ![Face](README_images/Valentino_Rossi_2010_Qatar.jpg)

``` sh
 $ export image_url=https://upload.wikimedia.org/wikipedia/commons/8/81/Valentino_Rossi_2010_Qatar.jpg
``` 

And invoke the API (this is a different endpoint)

``` sh
  $ curl -X GET "https://watson-api-explorer.mybluemix.net/visual-recognition/api/v3/detect_faces?url=${image_url}&version=2016-05-20"
```

And details about the person in the picture.   
***Note: if you don't know Valentino Rossi, please pause here and go and watch some awesome motorcycle race"***

``` sh
{
    "images": [
        {
            "faces": [
                {
                    "age": {
                        "max": 24,
                        "min": 18,
                        "score": 0.677153
                    },
                    "face_location": {
                        "height": 395,
                        "left": 148,
                        "top": 172,
                        "width": 316
                    },
                    "gender": {
                        "gender": "MALE",
                        "score": 0.993307
                    },
                    "identity": {
                        "name": "Valentino Rossi",
                        "score": 0.880797,
                        "type_hierarchy": "/riders/valentino rossi"
                    }
                }
            ],
            "resolved_url": "https://upload.wikimedia.org/wikipedia/commons/8/81/Valentino_Rossi_2010_Qatar.jpg",
            "source_url": "https://upload.wikimedia.org/wikipedia/commons/8/81/Valentino_Rossi_2010_Qatar.jpg"
        }
    ],
    "images_processed": 1
}
```

Now let's move on and see how to teach something to Watson

## Creating a custom classifier.

It's possible to extend the 'knowledge' of Visual Recognition service creating classifiers and training the service feeding it with images. 
A classifier is a container of classes, it's a convenient way to organize different topics. 
Classes are specific objects or caracteristics we want to be regognized.
Training is done uploading to Visual Recognition an archive (zip) of pictures for each class. Recommended number of images per class is 150-200, above 5000 there's no real improvement. Training doesn't have to be done in one go.  

For this example I am going to create a classifier for motorbike tyres, and two classes, one for new tyres the other one for used tyres. I then passed a set of negative picures (not new or used motorbike tyres, such as bicycle or truck tyres).

Let's use some APIs for this.

``` sh
  curl -X POST  -F "bikeTyreUsed_positive_examples=@moto_tyre_used.zip" -F "bikeTyreNew_positive_examples=@moto_tyre_new.zip" -F "bikeTyre_negative_examples=@moto_tyre_used.zip" -F "name=bikeTyres" "https://watson-api-explorer.mybluemix.net/visual-recognition/api/v3/classifiers?api_key=${api_key}&version=2016-05-20"
```
The service return some details about the classifier (important to note down the classifier_id) and the status of the classifier. In the first response the status will be 'training'. Depending on the number of images to analyze, the training can take from few minutes to few hours.

``` sh
{
    "classifier_id": "bikeTyres_77196102",
    "name": "bikeTyres",
    "owner": "f7ffaf4b-1ec4-4ffe-a873-a8a91197e60a",
    "status": "training",
    "created": "2017-08-10T04:06:48.573Z",
    "classes": [
        {"class": "bikeTyreUsed"},
        {"class": "bikeTyreNew"}
    ]
}
```

To check the status of a classifier (training completed) just send a GET request to the same endpoint.

``` sh
  $ curl -X GET "https://watson-api-explorer.mybluemix.net/visual-recognition/api/v3/classifiers/bikeTyres_77196102?api_key=${api_key}&version=2016-05-20"
```

Now the classifier is ready to be used

``` sh
{
    "classifier_id": "bikeTyres_77196102",
    "name": "bikeTyres",
    "owner": "f7ffaf4b-1ec4-4ffe-a873-a8a91197e60a",
    "status": "ready",
    "created": "2017-08-10T04:06:48.573Z",
    "classes": [
        {"class": "bikeTyreUsed"},
        {"class": "bikeTyreNew"}
    ]
}
```

STUFF

The free tier of the service allow 1 classifier, to delete the current classifier and upload a new one it's possible to use a specific API

``` sh
 $ curl -X DELETE "https://watson-api-explorer.mybluemix.net/visual-recognition/api/v3/classifiers/fruitbowls_1692746911?api_key=39ce25c61452cadf066ed34c612cbf59530232fc&version=2016-05-20"

```


