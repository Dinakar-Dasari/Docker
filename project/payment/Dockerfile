FROM python:3.9.23-alpine3.22
WORKDIR /opt/server
COPY requirements.txt .
COPY payment.ini .
COPY *.py .
RUN pip3 install -r requirements.txt
ENV CART_HOST="cart" \
    CART_PORT="8080" \
    USER_HOST="user" \
    USER_PORT="8080" \
    AMQP_HOST="rabbitmq"    
## Remaining env will give in compose file. To check whethr it works or not    
CMD ["uwsgi", "--ini", "payment.ini"]