# Overview
Distribute ASCII arts generation with [ascii-magic](https://github.com/LeandroBarone/python-ascii_magic) across multiple servers for scalability.

![image](https://raw.githubusercontent.com/EugeneSqr/asciilograph/assets/demo.gif)

# Development setup
Make sure to have the `.env` file with all the settings:
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
