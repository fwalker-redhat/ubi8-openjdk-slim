# Copyright 2019 Red Hat
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
# ------------------------------------------------------------------------

FROM registry.redhat.io/ubi8/ubi-minimal:latest

###### OpenJDK 8 / Prometheus / Jolokia
COPY openjdk /opt/openjdk

USER root

RUN microdnf --setopt=tsflags=nodocs install -y java-1.8.0-openjdk-devel prometheus-jmx-exporter jolokia-jvm-agent\
    && microdnf clean all \
    && rpm -q java-1.8.0-openjdk-devel prometheus-jmx-exporter jolokia-jvm-agent \
    && mkdir /deployments && chown 1001:root /deployments && chmod u+s /deployments

ENV \
    JAVA_HOME="/usr/lib/jvm/java-1.8.0" \
    JAVA_VENDOR="openjdk" \
    JAVA_VERSION="1.8.0" \
    AB_PROMETHEUS_JMX_EXPORTER_CONFIG="/opt/openjdk/prometheus/etc/jmx-exporter-config.yaml" \
    AB_JOLOKIA_AUTH_OPENSHIFT="true" \
    AB_JOLOKIA_HTTPS="true" \
    AB_JOLOKIA_PASSWORD_RANDOM="true" \
    JOLOKIA_VERSION="1.6.2"

LABEL \
    jdk.product="openjdk"  \
    jdk.product.openjdk.version="1.8.0"  \
    jdk.product.version="1.8.0" \
    io.fabric8.s2i.version.jolokia="1.6.2-redhat-00002" \
    io.cekit.version="3.6.0"  \
    io.k8s.display-name="OpenJDK 8 Java Applications"  \
    io.openshift.tags="java"

EXPOSE 8778 8080 8443
#####

# Clear package manager metadata
USER root
RUN microdnf clean all && [ ! -d /var/cache/yum ] || rm -rf /var/cache/yum

# Define the user
USER 1001
# Define the working directory
WORKDIR /deployments
# Define run cmd
CMD ["/opt/openjdk/run"]