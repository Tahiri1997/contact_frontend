
docker build -t vue-frontend .
docker run -d -p 8080:8080 --name vue-frontend vue-frontend
