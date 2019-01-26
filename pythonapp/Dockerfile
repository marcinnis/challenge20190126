FROM python:2.7-alpine
COPY requirements.txt /
COPY app.py /
ENV PATH=$PATH:/home/app/.local/bin

RUN addgroup -g 1000 -S app && \
    adduser -u 1000 -S app -G app
WORKDIR /
USER app
RUN pip install -r requirements.txt --user
CMD ["python", "app.py"]
