FROM python:3.11
WORKDIR /usr/src/app
COPY . . 
ENV REDIS_URL  redis://127.0.0.1:6379/0
ENV HTTP_PORT 5000
ENV JWT_SECRET supersecret
RUN pip install --no-cache-dir -r requirements.txt
RUN export PATH=$PATH:$HOME/.local/bin
CMD ["python", "main.py"]
EXPOSE 6379
EXPOSE 5000