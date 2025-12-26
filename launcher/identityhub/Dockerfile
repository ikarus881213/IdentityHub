FROM eclipse-temurin:24.0.2_12-jre-alpine

# Optional JVM arguments, such as memory settings
ARG JVM_ARGS=""
RUN apk --no-cache add curl

WORKDIR /app

# Copy the JAR file from its correct location
COPY launcher/identityhub/build/libs/identity-hub.jar /app/app.jar

# Copy the configuration file from the project root
COPY dataspaceconnector-configuration.properties /app/config.properties

EXPOSE 8181 8182 10001

ENV WEB_HTTP_PORT="8181"
ENV WEB_HTTP_PATH="/api"

HEALTHCHECK --interval=5s --timeout=5s --retries=10 CMD curl --fail http://localhost:8181/api/check/health || exit 1

# Use "exec" for graceful termination (SIGINT) to reach JVM.
# ARG can not be used in ENTRYPOINT so storing value in an ENV variable
ENV ENV_JVM_ARGS=$JVM_ARGS
ENTRYPOINT [ "sh", "-c", "exec java $ENV_JVM_ARGS -Dedc.fs.config=/app/config.properties -jar app.jar"]