
# Custom Nginx Docker Image with Additional Modules

This repository provides a Docker image based on Nginx, precompiled with the following modules:

- `headers-more-nginx-module`
- `naxsi`
- `ModSecurity-nginx`
- `ngx_brotli`
- `njs`
- `zlib:1.3.1`
- `openssl:3.3.2`

The image is built from a multi-stage Dockerfile, ensuring a small final image size while supporting essential security and performance features.

## Features

1. **Headers More Nginx Module** - Allows modifying HTTP request and response headers.
2. **Naxsi** - A high-performance Nginx web application firewall.
3. **ModSecurity-nginx** - A module that provides a robust security firewall.
4. **Brotli Compression** - Supports Brotli compression for better performance.
5. **njs** - Enables scripting with JavaScript (njs) for various customizations.
6. **zlib** - Compression library for Nginx.
7. **OpenSSL** - Enables HTTPS support and secure connections.

## Docker Image Structure

The image is built using two Dockerfiles:

1. `dockerfile-base`: This is the base image that compiles all required dependencies and modules for Nginx.
2. `dockerfile-custom`: This uses the base image and sets up the final environment for Nginx, including your custom `nginx.conf`.

## Building the Images

### Base Image

To build the base image, use the following commands:

- **Docker**:

    ```bash
    docker build -f dockerfile-base -t custom-nginx-base:latest .
    ```

- **Podman**:

    ```bash
    podman build -f dockerfile-base -t custom-nginx-base:latest .
    ```

- **Nerdctl**:

    ```bash
    nerdctl build -f dockerfile-base -t custom-nginx-base:latest .
    ```

### Custom Image

Once the base image is built, build the custom image using:

- **Docker**:

    ```bash
    docker build -f dockerfile-custom -t custom-nginx:latest .
    ```

- **Podman**:

    ```bash
    podman build -f dockerfile-custom -t custom-nginx:latest .
    ```

- **Nerdctl**:

    ```bash
    nerdctl build -f dockerfile-custom -t custom-nginx:latest .
    ```

### Running the Container

After building the image, you can run the Nginx container:

```bash
docker run -d -p 80:80 -p 443:443 custom-nginx:latest
```

This will start Nginx on ports 80 (HTTP) and 443 (HTTPS), ready to serve requests with the additional modules and configurations enabled.

### Configuration

You can modify the Nginx configuration by editing the `nginx.conf` file. This file is responsible for setting up various location blocks, reverse proxy settings, and enabling specific features such as Brotli compression, GeoIP blocking, and header manipulation.

Here is a summary of some features available in the provided `nginx.conf`:

- **Reverse Proxy**: The configuration supports reverse proxying requests to other services using the `/proxy` endpoint.
- **FastCGI**: It includes a location block for handling PHP files through FastCGI.
- **uWSGI**: Configured to handle uWSGI requests, useful for Python applications.
- **SCGI**: Supports SCGI protocol for certain application use cases.
- **Headers More**: Demonstrates usage of the `Headers More` module to set custom HTTP headers.
- **Brotli and Gzip Compression**: Compression is enabled for better performance on client requests.
- **GeoIP Block**: Basic GeoIP setup to block access from specific countries (example: blocking China).
- **Custom Error Pages**: Defined a custom 404 error page.

### Testing the Configuration

You can quickly test the provided features by accessing specific endpoints:

- Visit `/headers` to test custom headers using the Headers More module.
- Visit `/proxy` for reverse proxy testing.
- Visit `/perl` to test perl module.
- Use `/geoip` for GeoIP-based access control.

### Customizing

Feel free to adjust the `nginx.conf` or modify the Dockerfiles to include any additional features or settings specific to your environment.

## Contributing

If you encounter any issues or want to suggest improvements, feel free to open an issue or submit a pull request.

## License

This project is licensed under the Apache 2.0 License. See the `LICENSE` file for details.