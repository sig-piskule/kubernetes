# Copyright 2023 The Kubernetes Authors.
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

FROM golang:1.23.0-bullseye@sha256:ea90fd4edb6db8b5cf8751fe5eeb08a25549090f48d08c7f3fa91dae648308b6 as builder

WORKDIR /workspace

# Copy the source
COPY apimachinery/ apimachinery/
COPY kms/ kms/
WORKDIR /workspace/kms/internal/plugins/_mock

ARG TARGETARCH
ARG TARGETPLATFORM
RUN CGO_ENABLED=1 GOOS=linux GOARCH=${TARGETARCH} GO111MODULE=on go build -a -o mock-kms-plugin plugin.go

FROM alpine:latest@sha256:f59abe656b692d901e184be3445e084786602108599d2a51d229c0f5a48c71bf

RUN apk add --update --no-cache ca-certificates gcompat
RUN apk add --no-cache softhsm

COPY --from=builder /workspace/kms/internal/plugins/_mock/mock-kms-plugin /usr/local/bin/mock-kms-plugin

ENTRYPOINT [ "mock-kms-plugin" ]
