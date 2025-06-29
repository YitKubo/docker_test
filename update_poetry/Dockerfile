FROM python:3.11-slim
WORKDIR /update_poetry
COPY requirements.txt ./
RUN pip install --no-cache-dir poetry
COPY . .
#CMD ["sleep", "infinity"]