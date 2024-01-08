# Overview
Distribute ASCII arts generation with [ascii-magic](https://github.com/LeandroBarone/python-ascii_magic) across multiple servers for scalability.

![image](https://raw.githubusercontent.com/EugeneSqr/asciilograph/assets/demo.gif)

## High level design
Here is how multiple pieces tie together:
![image](https://raw.githubusercontent.com/EugeneSqr/asciilograph/assets/asciilograph.jpg)

[asciilograph-api](https://github.com/EugeneSqr/asciilograph-api) is a RESTful API which accepts HTTP requests from clients. It streams image bytes to a file server and enqueues image processing via a message broker (rabbitmq). Multiple instances of [asciilograph-worker](https://github.com/EugeneSqr/asciilograph-worker) pick up euqueued processing tasks and do all the job using awesome [ascii-magic](https://github.com/LeandroBarone/python-ascii_magic) library. After the processing is complete `asciilograph-worker` reports results back to `asciilograph-api` with a message enqueued to a dedicated callback queue. The `asciilograph-api` then responds back to the client with the result text.

# Development setup
Make sure to have the `.env` file with all the settings in the `./asciilograph` directory alongside `docker-compose.yml`:
```yaml
RABBITMQ_HOST=rabbitmq
RABBITMQ_USER=admin
RABBITMQ_PASSWORD=admin
RABBITMQ_IMAGE_PROCESSING_QUEUE=image_processing
RABBITMQ_CONNECTION_POOL_SIZE=100
FILESERVER_ADDRESS=fileserver
FILESERVER_USER=fsuser
FILESERVER_PASSWORD=fspassword
```

then run:
```bash
docker-compose up
```

## Running the conversion
Start issuing HTTP requests with binary data included. Don't use multipart uploads, they aren't streamable.

```bash
curl -X POST 'http://localhost:8000/ascii_arts' --data-binary '@test.jpg'
```
