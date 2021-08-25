# Build NGINX from source example

# Again, call in ubuntu image for base, gives access to ubuntu shell
FROM ubuntu:latest

# The update and install files are required for NGINX build
# apt-get clean && rm, gets rid of excess cached
RUN apt-get update && \
    apt-get install build-essential\ 
                    libpcre3 \
                    libpcre3-dev \
                    zlib1g \
                    zlib1g-dev \
                    libssl1.1 \
                    libssl-dev \
                    -y && \
    apt-get clean && rm -rf /var/lib/apt/lists/*
# We have this file in the folder already, copy it inside the image.
COPY nginx-1.19.2.tar.gz .
# RUN the file to extract its contents, then delete the file, leaving behind contents.
RUN tar -xvf nginx-1.19.2.tar.gz && rm nginx-1.19.2.tar.gz
# cd into contents and install software from source code (can read more on this later if interested)
RUN cd nginx-1.19.2 && \
    ./configure \
        --sbin-path=/usr/bin/nginx \
        --conf-path=/etc/nginx/nginx.conf \
        --error-log-path=/var/log/nginx/error.log \
        --http-log-path=/var/log/nginx/access.log \
        --with-pcre \
        --pid-path=/var/run/nginx.pid \
        --with-http_ssl_module && \
    make && make install
# build and installation is completed, remove the directory
RUN rm -rf /nginx-1.19.2
# complete the image build by starting NGINX in single process mode
CMD ["nginx", "-g", "daemon off;"]