# build custom nginx image

# Bring in ubuntu as base image
FROM ubuntu:latest
# layer on PORT 80 command; note: still need to publish when running container
EXPOSE 80

#update and install nginx package
RUN apt-get update && \
    apt-get install nginx -y && \
# clear the cache to reduce size, remove excess
    apt-get clean && rm -rf /var/lib/apt/lists/*

# This is written in 'exec' form.  nginx refers to the NGINX executable
# -g and daemon off are options for NGINX (read up on later)
CMD ["nginx", "-g", "daemon off;"]


## build the image using:
## docker image build --tag custom-nginx:packaged .
## use --tag to give the image a custom name, otherwise it'll come out with random string
# custom-name = the repository
# packaged = the tag
# compare this to:          ubuntu:latest
                    # (repository) : (tag)
# When removing single items, if they've been tagged, then the repo and tag need to be called
# docker image rm custom-name:packaged