# Copyright 2019 The Kubernetes Authors.
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


FROM golang:1.13.10-alpine3.10 as builder
WORKDIR /go/src/sigs.k8s.io/azuredisk-csi-driver
ADD . .
ARG TARGETARCH
ARG TARGETOS
ARG LDFLAGS
RUN CGO_ENABLED=0 GOOS=$TARGETOS GOARCH=$TARGETARCH go build -a -ldflags "${LDFLAGS:--X sigs.k8s.io/azuredisk-csi-driver/pkg/azuredisk.driverVersion=latest}" -o _output/azurediskplugin ./pkg/azurediskplugin

FROM us.gcr.io/k8s-artifacts-prod/build-image/debian-base-amd64:v2.1.0
COPY --from=builder /go/src/sigs.k8s.io/azuredisk-csi-driver/_output/azurediskplugin /azurediskplugin
RUN clean-install util-linux e2fsprogs mount ca-certificates udev xfsprogs

LABEL maintainers="andyzhangx"
LABEL description="Azure Disk CSI Driver"

ENTRYPOINT ["/azurediskplugin"]
