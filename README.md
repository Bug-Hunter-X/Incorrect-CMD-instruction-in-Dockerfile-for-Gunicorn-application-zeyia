# Incorrect CMD instruction in Dockerfile for Gunicorn application

This repository demonstrates a common error in Dockerfiles when using Gunicorn to serve a Python application.

## The Bug

The original `Dockerfile` uses a simple `CMD` instruction that is not suitable for applications using Gunicorn:

```dockerfile
CMD ["python3", "app.py"]
```

This command attempts to start the app.py directly using python3. However, applications using Gunicorn need to be started differently. For this reason, this command will likely result in a process that ends quickly or other runtime errors.

## The Solution

The solution is to specify the Gunicorn command explicitly as shown in the fixed `Dockerfile`.

```dockerfile
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "app:app"]
```

This will start the Gunicorn server, which correctly handles the application's lifecycle and allows for concurrent requests.

## Steps to Reproduce

1.  Clone this repository.
2.  Build the original Dockerfile: `docker build -t myapp .`
3.  Run the container: `docker run -p 8000:8000 myapp`.  This will show the issue.
4.  Build the fixed Dockerfile: `docker build -t myapp_fixed -f Dockerfile_fixed .`
5.  Run the fixed container: `docker run -p 8000:8000 myapp_fixed`. This will start the application properly.

Note: Requires an `app.py` file (example provided).