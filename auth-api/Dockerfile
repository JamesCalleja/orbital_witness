FROM python:3.11
WORKDIR /usr/src/app
COPY . . 
ENV HTTP_PORT 5000
ENV JWT_SECRET supersecret
RUN pip install --no-cache-dir -r requirements.txt
RUN export PATH=$PATH:$HOME/.local/bin
CMD ["python", "main.py"]
EXPOSE 5000