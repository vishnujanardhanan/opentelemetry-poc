Run OpenSearch
-=============
docker run \
-p 9200:9200 -p 9600:9600 \
-e "discovery.type=single-node" \
-e "plugins.security.disabled=true" \
opensearchproject/opensearch:2.2.0

Test OpenSearch
-=============
curl -XGET http://localhost:9200 -u 'admin:admin' --insecure

Run Data prepper
-=============
docker run --name data-prepper \
    -v /Users/vishnu/git/opentelemetry-poc/javaagent/collector/pipeline.yaml:/usr/share/data-prepper/pipelines.yaml \
    -v /Users/vishnu/git/opentelemetry-poc/javaagent/collector/data-prepper.yaml:/usr/share/data-prepper/data-prepper-config.yaml \
    opensearchproject/data-prepper:latest



Run otel Collector
=================
docker run --rm -p 4317:4317 -p 55680:55680 -p 8889:8888 \
--name awscollector public.ecr.aws/aws-observability/aws-otel-collector:latest \
-v /Users/vishnu/git/opentelemetry-poc/javaagent/collector/otel-config.yaml:/etc/otel-agent-config.yaml \
      --config /etc/otel-agent-config.yaml;

Run Application
==============
../gradlew bootJar
docker-compose up --build
