FROM nginx:latest
WORKDIR /app
RUN useradd -r -u 1001 appuser && \
    chown -R appuser /app

COPY /nginx.conf /etc/nginx/nginx.conf
EXPOSE 8000
