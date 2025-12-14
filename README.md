# olgtp-stack

App-> OTel Agent -> OTel Collector -> Prometheus
App-> OTel Agent -> OTel Collector -> Loki
App-> OTel Agent -> OTel Collector -> Tempo -> Grafana


Logs → OTel Collector → Loki → Grafana
Metrics → OTel Collector → Prometheus → Grafana
Traces → OTel Collector → Tempo → Grafana


./gradlew clean build --refresh-dependencies
./gradlew dependencies --configuration runtimeClasspath | grep slf4j

docker-compose down -v
docker-compose pull
docker-compose up -d


Tempo Control : curl http://localhost:3200/ready
Collector Metrics : curl http://localhost:8888/metrics



Logs :
docker logs loki
docker logs otel-collector
docker ps | grep 8888

#OTel Agent
mkdir otel
cd otel
curl -L -o opentelemetry-javaagent.jar \
https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/download/v2.5.0/opentelemetry-javaagent.jar

# Grafana
docker dizini altında grafana klasörü bulunur. Buradaki yaml dosyası grafanaya her açılışta 
manuel olarak loki,prometheus ve tempo yu ekleme zorunluluğunu kaldırır buradan okur.
# Calistirma

java -javaagent:otel/opentelemetry-javaagent.jar -jar build/libs/lgtpstack-0.0.1-SNAPSHOT.jar

docker olarak çalıştırmada
docker-compose up 
## ortak network kurulur
docker network create observability

networks: [observability] ile aynı networkte çalışırlar. DNS isimleri çözülür.
