FROM donaldrich/builder:latest  AS waypoint-builder

# ENV WAYPOINT_VERSION=0.1.3
# RUN wget https://releases.hashicorp.com/waypoint/${WAYPOINT_VERSION}/waypoint_${WAYPOINT_VERSION}_linux_amd64.zip
# RUN unzip waypoint_${WAYPOINT_VERSION}_linux_amd64.zip

COPY --from=hashicorp/waypoint:latest /usr/bin/waypoint /usr/local/bin/waypoint
RUN upx /usr/local/bin/waypoint

FROM alpine

COPY --from=waypoint-builder  /usr/local/bin/waypoint /usr/local/bin/waypoint
# validate
COPY --from=donaldrich/function:task      /usr/local/bin/tusk /usr/local/bin/tusk
COPY ./Dockerfile ./Dockerfile
COPY ./tusk.yml ./tusk.yml
RUN tusk validate
ENTRYPOINT [ "tusk" ]
