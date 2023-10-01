# API Documentation

- Base URL: https://hng5.onrender.com

### Authentication
No authentication required for the provided endpoints.

### Start Video Streaming/Recording

- Request:
    Endpoint: https://hng5.onrender.com/api/video/start
    Method: POST
    Headers: None
    Body:
        - blob: video
        - videoId: <Unique Video ID>

- Response:
    Status: 200 OK
    Body:
        {
            "status": true,
            "msg": "Video Streaming in progress"
        }

### Stop Video Streaming/Recording

- Request:
    Endpoint: https://hng5.onrender.com/api/video/end/<videoId>
    Method: GET
    Headers: None
    Usage:
        - Send a GET request with the videoId as a parameter.

- Response:
    Status: 200 OK
    Body:
        {
            "status": true,
            "msg": "Video Streaming stopped"
        }
    Status: 404 Not Found
    Body:
        {
            "status": false,
            "msg": "Video not found"
        }

### Get Video by ID

- Request:
    Endpoint: https://hng5.onrender.com/api/video/<videoId>
    Method: GET
    Headers: None

- Response:
    Status: 200 OK
    Headers:
        Status Code: 200 OK (If the video exists)
        Status Code: 404 Not Found (If the video does not exist)
    Body: The binary video content.

    Note: Video streaming is supported with the use of the Range header.

### Get All Videos

- Request:
    Endpoint: https://hng5.onrender.com/api/video/
    Method: GET
    Headers: None

- Response:
    Status: 200 OK
    Body: List of video information.

    Example:
    ```
    {
        "status": true,
        "message": "Video fetched successfully",
        "data": [
            {
                "videoId": "unique_video_id",
                "video": "https://hng5.onrender.com/media/files/unique_video_id.webm",
                "createdAt": "Timestamp"
            },
        ]
    }
    ```

### Stream Uploaded Video

- Request:
    Endpoint: https://hng5.onrender.com/api/video/stream/<videoId>
    Method: GET
    Headers: None
    Usage:
        - Send a GET request with the videoId as a parameter.
        - It serves the requested video file, supports video range requests.

- Response:
    Status: 200 OK (If the video exists)
    Status: 404 Not Found (If the video does not exist)
    Headers:
        Status Code: 200 OK (If the video exists)
        Status Code: 206 Partial Content (If video streaming with range)
        Content-Range (if streaming): Indicates the range of bytes being sent.
        Content-Length: The length of the video file.
        Content-Type: The content type (video/mp4).
    Body: The binary video content.

    Note: Video streaming is supported with the use of the Range header.
