FROM redis:latest
WORKDIR /usr/src/app
COPY . . 
ENV REDIS_URL  redis://127.0.0.1:6379/0
RUN apt-get update && apt-get install -y python3-pip
RUN pip install --no-cache-dir -r requirements.txt
RUN export PATH=$PATH:$HOME/.local/bin
CMD ["python", "main.py"]
EXPOSE 6379