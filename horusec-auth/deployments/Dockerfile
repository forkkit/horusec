# Copyright 2020 ZUP IT SERVICOS EM TECNOLOGIA E INOVACAO SA
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

FROM golang:alpine AS builder

RUN apk update && apk add --no-cache git build-base

ADD . /horusec

WORKDIR /horusec

RUN go get -t -v -d ./...

RUN GOOS=linux go build -a -o horusec-auth-main ./horusec-auth/cmd/app/main.go

RUN GRPC_HEALTH_PROBE_VERSION=v0.3.2 && \
    wget -qO/bin/grpc_health_probe https://github.com/grpc-ecosystem/grpc-health-probe/releases/download/${GRPC_HEALTH_PROBE_VERSION}/grpc_health_probe-linux-amd64 && \
    chmod +x /bin/grpc_health_probe

FROM alpine

COPY --from=builder /horusec/horusec-auth-main .
COPY --from=builder /bin/grpc_health_probe ./grpc_health_probe

ENTRYPOINT ["./horusec-auth-main"]
