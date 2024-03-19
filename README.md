# Image Processing Application

This is a dockerized image processing application that consists of several microservices working together. The application allows users to upload images, processes them using an image processing service, and stores the processed images in an S3-compatible storage. The application also maintains a provenance database to keep track of the image processing history.

## Services

The application is composed of the following services:

- `upload`: Accepts image uploads and sends them to the image processing service via RabbitMQ.
- `imageproc-runner`: Receives images from the upload service, processes them using the `imageproc` service, and uploads the processed images to S3.
- `imageproc`: Performs the actual image processing.
- `provenancedb`: Stores the provenance information of the processed images in a CouchDB database.
- `couchdb`: The database used by the `provenancedb` service.
- `rabbitmq`: The message broker that facilitates communication between the services.

## Configuration

To configure the application, create a `.env` file in the same directory as the `docker-compose.yml` file with the following variables:

```
UPLOAD_IMAGE=<upload-service-image>
RABBITMQ_USER=<rabbitmq-username>
RABBITMQ_PASSWORD=<rabbitmq-password>
S3_URL=<s3-url>
S3_INPUT_BUCKET=<s3-input-bucket>
S3_OUTPUT_BUCKET=<s3-output-bucket>
S3_ACCESS_KEY=<s3-access-key>
S3_SECRET_KEY=<s3-secret-key>
IMAGEPROC_RUNNER_IMAGE=<imageproc-runner-image>
IMAGEPROC_IMAGE=<imageproc-image>
AMQP_EXCHANGE_PROV=<provenance-exchange-name>
PROVENANCEDB_IMAGE=<provenancedb-image>
COUCHDB_USER=<couchdb-username>
COUCHDB_PASSWORD=<couchdb-password>
COUCHDB_DB=<couchdb-database-name>
```

Replace the placeholders (`<...>`) with the appropriate values for your setup.

## Running the Application

To start the application, run the following command in the directory containing the `docker-compose.yml` file:

```
docker-compose up -d
```

This will start all the services in detached mode.

## Accessing the Application

- The `upload` service is accessible at `http://localhost:8000`.
- The RabbitMQ management interface is accessible at `http://localhost:15672`.
- The CouchDB interface is accessible at `http://localhost:5984`.

Make sure to configure the necessary ports and credentials as specified in the `.env` file.
