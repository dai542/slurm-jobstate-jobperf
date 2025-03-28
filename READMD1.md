网络不好，就把 1 文件夹下的所有文件放到主目录，然后修改`~/jobperf-main/cmd/jobperf/fetchVendor.sh`为

```
#!/bin/bash

set -e

BUILD_DIR=$(mktemp -d)
echo "building in $BUILD_DIR..."
cp ~/jobperf-main/htmx.min.js "$BUILD_DIR/"
cp ~/jobperf-main/ws.min.js "$BUILD_DIR/"
cp ~/jobperf-main/chart.umd.min.js "$BUILD_DIR/"
cp ~/jobperf-main/luxon.min.js "$BUILD_DIR/"
cp ~/jobperf-main/chartjs-adapter-luxon.umd.min.js "$BUILD_DIR/"

sha256sum -c << EOF
aa4f88f51685e7b22e9bd54b727d196168ee85b8ce5ce082c0197ec676976b5b  $BUILD_DIR/chartjs-adapter-luxon.umd.min.js
e9b0f875106021fb3d58120ad8ebdd3e7d32135a4452fd8918c72ac6475f2bd3  $BUILD_DIR/chart.umd.min.js
96a334a9570a382cf9c61a1f86d55870ba1c65e166cc5bcae98ddd8cdabeb886  $BUILD_DIR/htmx.min.js
ecd426d1b86f0c92a8b0bf1dfba6604a2d8bc59088700fd30f4f3b18b1013bd3  $BUILD_DIR/luxon.min.js
db6a44355128d5f795e8116df902dfe640b2598bbebd4fa655416b9fe734da14  $BUILD_DIR/ws.min.js
EOF

echo "Downloaded and verified."

cat \
    "$BUILD_DIR/chart.umd.min.js" <(echo) \
    "$BUILD_DIR/luxon.min.js" <(echo) \
    "$BUILD_DIR/chartjs-adapter-luxon.umd.min.js" <(echo) \
    "$BUILD_DIR/htmx.min.js" <(echo) \
    "$BUILD_DIR/ws.min.js" <(echo) \
    > vendor/vendor.js
```
