FROM golang:1.22-alpine AS builder

WORKDIR /usr/src/app

COPY . .

RUN go mod download

# Note: Keeping your exact build command
RUN go build -o /go/bin/product-catalog ./

# CHANGE THIS LINE: Use the OpenTelemetry image as the base instead of plain alpine
FROM ghcr.io/open-telemetry/demo:2.2.0-product-catalog AS release

WORKDIR /usr/src/app

COPY ./products ./products

# This overwrites the old binary inside the OTel image with your newly built one
COPY --from=builder /go/bin/product-catalog ./

ENV PRODUCT_CATALOG_PORT=8088
ENTRYPOINT ["./product-catalog"]

