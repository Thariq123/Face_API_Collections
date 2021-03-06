---
description: This section contains documentation on Liveness API usage and functions.
---

# Liveness API

![](../.gitbook/assets/risetai\_logo.72c56424.png)

## **Single Liveness API**

### **POST `/liveness-api/verify-face-liveness`**

This API predicts the liveness of the face from two types active and passive. Given a requested `parameter`, will return `True` or `False` depending on the API's prediction.

#### **Request**

#### **`Headers`**

| KEY             | VALUE               |
| --------------- | ------------------- |
| **Accesstoken** | `oauth Accesstoken` |

#### **`Body`**

```json
{      
    "facegallery_id": "riset.ai@production",
    "user_id":        "risetai1234",
    "image":          "/9j/4AAQSkZJRgABAQEASABIAAD/4QBMRXhpZgAA...",
    "trx_id":         "alphanumericalstring1234",
    "face_parameters" : {
        "spoof_detection" : true,
        "angle"           : "face left",
        "eyes"            : "closed",
        "mouth"           : "open"
    }
}
```

| Key                               | Type     | Description                                                                                                                                    | Possible Parameters                                             |
| --------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| `user_id`                         | `string` | Unique User/Face Identifier, **this is optional but recommended** (paired with `facegallery_id`)                                               | -                                                               |
| `facegallery_id`                  | `string` | Unique FaceGallery identifier, alphanumeric (eg. LocationName, CompanyName, etc), **this is optional but recommended** (paired with `user_id`) | -                                                               |
| `image`                           | `string` | Base64 encoded JPG or PNG image                                                                                                                | -                                                               |
| `trx_id`                          | `string` | Unique transaction identifier, for transaction logging and debugging purposes                                                                  | -                                                               |
| `face_parameters`                 | `array`  | contains requested parameters to match with API prediction, at least `one` `parameter` is needed                                               | -                                                               |
| `face_parameters.angle`           | `string` | "left" or "right", API will predict if the input `image` is the facing `left` or `right`, **this is optional**                                 | "face left", "face right", "face up", "face down", "face front" |
| `face_parameters.spoof_detection` | `bool`   | Specify whether to do `spoof_detection` or not, will default to `True` if none specified, **this is optional**                                 | true, false                                                     |
| `face_parameters.eyes`            | `string` | "open" or "closed", API will predict if the input `image` contains a face with `open` or `closed` eyes, **this is optional**                   | "open", "closed"                                                |
| `face_parameters.mouth`           | `string` | "open" or "closed", API will predict if the input `image` contains a face with `open` or `closed` mouth, **this is optional**                  | "open", "closed"                                                |

#### **Response**

#### **`Headers`**

| KEY              | VALUE              |
| ---------------- | ------------------ |
| **Content-Type** | `application/json` |

#### **`Body`**

```json
{
    "status": "200",
    "status_message": "Success",
    "liveness": "false",
    "match_status": {
        "verified": false,
        "is_real": true,
        "angle": false,
        "eyes": true,
        "mouth": false
    },
    "results": {
        "similarity": 0.1557,
        "masker": true,
        "is_spoof": false,
        "spoof_score": 0.0031,
        "roll": 1.48,
        "pitch": -3.25,
        "yaw": -13.38,
        "left_eye": "open",
        "left_eye_score": 1.0,
        "right_eye": "open",
        "right_eye_score": 0.9999,
        "mouth": "masked",
        "mouth_score": 0.0
    }
}
```

| Key                       | Type      | Description                                                                                                        |
| ------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------ |
| `status`                  | `string`  | Describing the condition of API hit                                                                                |
| `status_message`          | `string`  | The verbose message of API hit status                                                                              |
| `confidence_level`        | `float`   | Describe confidence of model, scale 0.0 to 1.0 (from 0% to 100% confidence)                                        |
| `verified`                | `boolean` | If confidence\_level above threshold, returns `True`                                                               |
| `match_status`            | `array`   | Contains result of requested `face_parameters`                                                                     |
| `match_status.angle`      | `boolean` | Returns `True` if API predicts the same result as requested `face_parameters.angle`, will only return if requested |
| `match_status.is_real`    | `boolean` | Returns `True` if face deemed `real`                                                                               |
| `match_status.eyes`       | `boolean` | Returns `True` if API predicts the same result as requested `face_parameters.eyes`, will only return if requested  |
| `match_status.mouth`      | `boolean` | Returns `True` if API predicts the same result as requested `face_parameters.mouth`, will only return if requested |
| `match_status.verified`   | `bool`    | If face considered verified, will return `True`                                                                    |
| `results`                 | `array`   | Contains a more detailed result of predicted face attributes                                                       |
| `results.similarity`      | `float`   | Face similarity score                                                                                              |
| `results.masker`          | `bool`    | If face wears masker, will return `True`                                                                           |
| `results.is_spoof`        | `string`  | If image is considered spoof, will return `True`, else `False`                                                     |
| `results.spoof_score`     | `float`   | Spoof detection score                                                                                              |
| `results.roll`            | `float`   | Predicted face roll degree                                                                                         |
| `results.pitch`           | `float`   | Predicted face pitch degree                                                                                        |
| `results.yaw`             | `float`   | Predicted face yaw degree                                                                                          |
| `results.left_eye`        | `string`  | If image contains opened left eye, will return `open`, else, `closed`                                              |
| `results.left_eye_score`  | `float`   | Predicted left\_eye score                                                                                          |
| `results.right_eye`       | `bool`    | If image contains opened right eye, will return `open`, else, `closed`                                             |
| `results.right_eye_score` | `float`   | Predicted right\_eye score                                                                                         |
| `results.mouth`           | `bool`    | If image contains open mouth, returns `open`. If closed mouth, returns `closed`, if masked returns `masked`        |
| `results.mouth_score`     | `float`   | Predicted mouth score                                                                                              |

## **Batch Liveness API**

### **POST `/liveness-api/batch-verify-face-liveness`**

This API predicts the liveness of the face from two typed active and passive from multiple images. Given a requested `parameter`, will return `True` or `False` depending on the API's prediction.

#### **Request**

#### **`Headers`**

| KEY             | VALUE               |
| --------------- | ------------------- |
| **Accesstoken** | `oauth Accesstoken` |

#### **`Body`**

```json
{  
    "facegallery_id"    : "riset.ai@production",
    "user_id"           : "risetai1234",
    "trx_id"            : "alphanumericalstring1234",
    "success_threshold" : 8,
    "batch"             : [
        {
            "image" : "/92fhfsuefhs73yf73sfs3...",
            "face_parameter" : "eyes open",
            "spoof_detection" : true,
            "face_verification" : true
        },
        {
            "image" : "/92fhfsuefhs73yf73sfs3...",
            "face_parameter" : "left",
            "spoof_detection" : false
        },
        {
            "image" : "/92fhfsuefhs73yf73sfs3...",
            "face_parameter" : "mouth open",
            "spoof_detection" : true,
            "face_verification" : false
        }
        
    ]
}
```

| Key                       | Type            | Description                                                                                                                                    | Possible Parameters                                                                                                       |
| ------------------------- | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `user_id`                 | `string`        | Unique User/Face Identifier, **this is optional but recommended** (paired with `facegallery_id`)                                               | -                                                                                                                         |
| `facegallery_id`          | `string`        | Unique FaceGallery identifier, alphanumeric (eg. LocationName, CompanyName, etc), **this is optional but recommended** (paired with `user_id`) | -                                                                                                                         |
| `success_threshold`       | `int`           | Minimal number of successful image liveness detection, will default to all images if none specified, **this is optional**                      | -                                                                                                                         |
| `trx_id`                  | `string`        | Unique transaction identifier, for transaction logging and debugging, **this is optional**                                                     | -                                                                                                                         |
| `batch`                   | `list of array` | List of array, containing `image`, `face_parameter`, and `spoof_detection`, **MAXIMAL: 8 batches**                                             | -                                                                                                                         |
| `batch.image`             | `string`        | `Base64` encoded `PNG` or `JPG` image                                                                                                          | -                                                                                                                         |
| `batch.face_parameter`    | `string`        | contains requested parameters to match with API prediction                                                                                     | "face left", "face right", "face up", "face down", "face front", "eyes open", "eyes closed", "mouth open", "mouth closed" |
| `batch.spoof_detection`   | `boolean`       | Specify whether to do spoof\_detection on `batch.image` or not, will default to `true` if none specified, **this is optional**                 | true, false                                                                                                               |
| `batch.face_verification` | `boolean`       | Specify whether to do face verification on `batch.image` or not, will default to `true` if none specified, **this is optional** true, false    |                                                                                                                           |

#### **Response**

#### **`Headers`**

| KEY              | VALUE              |
| ---------------- | ------------------ |
| **Content-Type** | `application/json` |

#### **`Body`**

```json
{
    "status": "200",
    "status_message": "Success",
    "liveness": "passed",
    "results": {
        "image1": {
            "match": true,
            "similarity": 0.7758,
            "verified": true,
            "masker": true,
            "is_spoof": true,
            "spoof_score": 0.0492,
            "left_eye": "open",
            "left_eye_score": 1.0,
            "right_eye": "open",
            "right_eye_score": 1.0
        },
        "image2": {
            "match": true,
            "similarity": 0.7758,
            "verified": true,
            "masker": true,
            "roll": 7.59,
            "pitch": 2.9,
            "yaw": 13.85
        },
        "image3": {
            "match": true,
            "masker": true,
            "is_spoof": true,
            "spoof_score": 0.0492,
            "mouth": "closed",
            "mouth_score": 0.9439
        }
    }
}
```

| Key                               | Type     | Description                                                                                 |
| --------------------------------- | -------- | ------------------------------------------------------------------------------------------- |
| `status`                          | `string` | Describing the condition of API hit                                                         |
| `status_message`                  | `string` | The verbose message of API hit status                                                       |
| `liveness`                        | `string` | Describes if the requested images are considered `"alive"`                                  |
| `results`                         | `array`  | Contains detailed info for each requested images                                            |
| `results.image_n`                 | `array`  | Contains detailed info of processed image, will be dynamic depending on requested parameter |
| `results.image_n.match`           | `bool`   | If image matches the requested parameter, will return `True`                                |
| `results.image_n.verified`        | `bool`   | If face considered verified, will return `True`                                             |
| `results.image_n.similarity`      | `float`  | Face similarity score                                                                       |
| `results.image_n.masker`          | `bool`   | If face wears masker, will return `True`                                                    |
| `results.image_n.is_spoof`        | `bool`   | If image is considered spoof, will return `True`, else, `False`                             |
| `results.image_n.spoof_score`     | `float`  | Spoof detection score                                                                       |
| `results.image_n.roll`            | `float`  | Predicted face roll degree                                                                  |
| `results.image_n.pitch`           | `float`  | Predicted face pitch degree                                                                 |
| `results.image_n.yaw`             | `float`  | Predicted face yaw degree                                                                   |
| `results.image_n.left_eye`        | `string` | If image contains opened left eye, will return `open`, else, `closed`                       |
| `results.image_n.left_eye_score`  | `float`  | Predicted left\_eye score                                                                   |
| `results.image_n.right_eye`       | `bool`   | If image contains opened right eye, will return `open`, else, `closed`                      |
| `results.image_n.right_eye_score` | `float`  | Predicted right\_eye score                                                                  |
| `results.image_n.mouth`           | `bool`   | If image contains opened mouth, will return `open`, else, `closed`                          |
| `results.image_n.mouth_score`     | `float`  | Predicted mouth score                                                                       |
